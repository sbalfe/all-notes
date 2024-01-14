# Blind 75 Leetcode

> Solutions - https://www.youtube.com/watch?v=KLlXCFG5TnA&list=PLot-Xpze53ldVwtstag2TL4HQhAnC8ATf
>
> useful resource - https://github.com/tajpouria/algorithms-and-data-structures-cheat-sheet

### 1. Two sum

> https://leetcode.com/problems/two-sum/

- Use a **hash map**.

```cpp
auto two_sum(const std::vector<int>& v, int target) {
    std::unordered_map<int, int> indices;
    int i = 0;
    for (const auto& item : v ){
        int complement = target - item;

        if (indices.find(complement) != indices.end()) {
            return std::make_pair(indices[complement], i);
        }
        indices[item] = i;
        ++i;
    }

    return std::make_pair(-1, -1); // Return a default value if no pair is found
}
```

### 2. Best Time to Buy and Sell Stock

> https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

- Use two 'pointers'.
- Sliding window.

```cpp
int main() {
    std::vector<int> prices {7,1,5,3,6,4};
    int maxProfit = 0;
    int minPrice = prices[0];

    for (int price : prices) {
        minPrice = std::min(minPrice, price);
        maxProfit = std::max(maxProfit, price - minPrice);
    }

    fmt::print("Max profit: {}\n", maxProfit);
}
```

### 3. Contains Duplicate

```cpp
auto ContainsDuplicate() {
    std::vector<int> nums {1, 2, 3, 1};
    std::unordered_set<int> seen;
    seen.reserve(nums.size());

    for (const auto &item : nums) {
        if (seen.find(item) != seen.end()) {
            return true;
        }
        seen.insert(item);
    }
    return false;
}

```

### 4. Product of array except self ==REVIEW==

> https://www.youtube.com/watch?v=bNvIQI2wAjk&list=PLot-Xpze53ldVwtstag2TL4HQhAnC8ATf&index=4

```cpp
auto ProductArrayExceptSelf(const std::vector<int>& nums) {
    
    std::vector<int> result (nums.size());
    std::vector<int> result2 (nums.size());
    std::inclusive_scan(nums.begin(), nums.end(), result.begin(), [](int x, int y){
        return x * y;
    });


    std::inclusive_scan(nums.rbegin(), nums.rend(), result2.begin(), [](int x, int y){
        return x * y;
    });
    print_range(result);
    print_range(std::views::reverse(result2));
    for (int i = 0; i < nums.size(); ++i){
        int right, left;
        if (i == 0){
            right = 1;
        } else {
            right = result[i-1];
        }
        if (i == nums.size()-1){
            left = 1;
        }
        else{
            left = std::views::reverse(result2)[i+1];
        }
        fmt::print("left  * right: {}\n", left * right);
    }
}
```

### 5. Maximum subarray

