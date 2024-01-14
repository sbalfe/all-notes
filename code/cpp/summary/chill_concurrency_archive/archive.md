### condition variables

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <array>
#include <random>
#include <ranges>
#include <algorithm>
#include "chilli_timer.hpp"
#include <fmt/format.h>
#include <cmath>
#include <limits>
#include <span>
#include <thread>
#include <mutex>
#include <condition_variable>

constexpr size_t DATASET_SIZE = 50'000'000;

void ProcessDataset(std::span<int> arr, int& sum){
    for (int x : arr) {
        constexpr auto limit = static_cast<double>(std::numeric_limits<int>::max());
        const auto y = static_cast<double>(x) / limit;
        sum += int(std::sin(std::cos(y)) * limit);
    }
}

auto GenerateDatasets(){
    std::minstd_rand rne;
    std::vector<std::array<int, DATASET_SIZE>> datasets {4};
    for (auto& arr: datasets){
        std::ranges::generate(arr, rne);
    }
    return datasets;
}

int DoBiggie() {

    auto datasets = GenerateDatasets();

    std::vector<std::jthread> workers;

    ChiliTimer timer;
    timer.Mark();
    std::mutex mtx;
    struct Value  {
        int v {0};
        char padding[60];
    };
    std::array<Value, 4> sum {0,0,0,0};

    auto t = timer.Peek();
    for (const int& i: std::views::iota(0,4)){
        workers.emplace_back(ProcessDataset, std::span{ datasets[i] }, std::ref(sum[i].v));
    }

    fmt::print("{} seconds\n", t);
    fmt::print("result: {}\n", std::accumulate(sum.begin(), sum.end(), 0, [](int acc, const Value& val){
        return acc + val.v;
    }));

    return 0;
}

class MasterControl {
public:

    // worker count stays the same in this case.
    MasterControl(int workerCount): lk {mtx}, workerCount{workerCount}{}

    // each worker thread calls this , we use this to then check have all our workers completed.
    void SignalDone(){
        {
            std::lock_guard lk { mtx };
            ++doneCount;
        }
        if (doneCount == workerCount){
            cv.notify_one();
        }
    }

    // Wait in the main thread until all threads increment the done count var.
    void WaitForAllDone(){
        cv.wait(lk, [this]{return doneCount == workerCount;});
        doneCount = 0;
    }

private:
    std::condition_variable cv;
    std::mutex mtx;
    std::unique_lock<std::mutex> lk;
    int workerCount;

    // shared memory
    int doneCount = 0;
};

class Worker {
public:
    Worker(MasterControl* pMaster): pMaster {pMaster}, thread{&Worker::Run, this} {}


    // Main thread calls this interface
    void SetJob(std::span<int> data, int* pOut) {
        {
            std::lock_guard lk{mtx};
            input = data;
            pOutput = pOut;
        }
        cv.notify_one();
    }

    void Kill(){
        {
            std::lock_guard lk {mtx};
            dying = true;
        }
        cv.notify_one();
    }

private:

    void Run(){

        // Only we can access the shared memory
        std::unique_lock lk {mtx};

        // Process various datasets
        while (true) {

            // wait until we are signalled, after waking up , evaluate the condition
            cv.wait(lk, [this] {
                return pOutput != nullptr || dying; // worked told to die, or we still have work to do.
            });

            // we reach here after waking up and have atomically acquired the mutex again.
            if (dying) {
                break;
            }

            ProcessDataset(input, *pOutput);
            pOutput = nullptr; // reset our pointer to null, we have modified the pointed to data the user points to already.
            input = {};
            pMaster->SignalDone();
        }
    }

    MasterControl* pMaster;
    std::jthread thread;
    std::condition_variable cv;
    std::mutex mtx;

    // Shared memory
    std::span<int> input;
    int* pOutput = nullptr; // our dataset to process
    bool dying = false;
};

int DoSmallies(){

    auto datasets = GenerateDatasets();

    ChiliTimer timer;

    struct Value  {
        int v {0};
        char padding[60];
    };

    std::array<Value, 4> sum {0,0,0,0};

    timer.Mark();

    constexpr size_t workerCount = 4;
    MasterControl mctrl {workerCount};

    std::vector<std::unique_ptr<Worker>> workerPtrs;
    for (size_t i = 0; i < workerCount; i++){
        workerPtrs.push_back(std::make_unique<Worker>(&mctrl));
    }

    constexpr auto subset_size = DATASET_SIZE / 10'000;

    for (size_t i = 0; i < DATASET_SIZE; i += subset_size) {

        for (const int& j: std::views::iota(0,4)){

            workerPtrs[j]->SetJob(std::span{ &datasets[j][i], subset_size}, &sum[j].v);
        }
        mctrl.WaitForAllDone();
    }


    const auto t = timer.Peek();
    fmt::print("{} seconds\n", t);
    fmt::print("result: {}\n", std::accumulate(sum.begin(), sum.end(), 0, [](int acc, const Value& val){
        return acc + val.v;
    }));

    for (auto& w: workerPtrs){
        w->Kill();
    }

    return 0;
}

int main(int argc, char** argv){
    if (argc > 1 && std::string {argv[1]} == "smol"){
        return DoSmallies();
    }
    return DoBiggie();
}
```

