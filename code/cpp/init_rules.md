```C++
#include <initializer_list>
#include <iostream>
void print(std::string var, int val){std::cout << var << " >>> " << val << std::endl;}


struct A5 {
    A5(){}
    int i;
};

class A5b {
    int i;
    friend int main();    /* set a friend as to enable private access*/
};

struct A5c {
    A5c(int i) : i(i){}
    A5c() = default;
    int i;
};

struct A5x {
    A5x(){}
    int i;
};

struct A5xd : public A5x {
    int j;
};

// honing in on aggregrate and what is not and what is
void Act5(){

    /*
        > A5 has a user provided construtor thus list initialisation changes

        1. List init of A5 causes , 2
        2. Non aggregate with empty braced-init-list causes value initialised
        , 3
        3. A user provided constructor was found, default constructor called which does nothing
        4. left unintialized
    */
    A5 a{};
    std::cout << a.i << std::endl;

    /* what is a user provided constructor
        struct A {
            A () = default;
        };

        > This is not a user provided constructor, its as if none were declared at all.
            A is aggregrate
        
        A::A() = default;

        > this is now a user provided constructor, its as if were wrote A(){} in the body
            A is not an aggregate thus uses constructors not aggregrate intilaisation 

        > c++ 20 aggreate types explitly state that no user declared constructors are to be found
    */

    /*
        A5c has a user provided constructor, thus not aggregate.
        
        1. List init of A5c, 2
        2. Non aggregate, class type with default constructor, and empty brace-init-list
            causes value initialisation , 3
        3. No user provided default constructor found, zero init object
        4. invoke default intilaisation if the implicitly defined default construtor is non trivial
            does nothign for this case.
    */
    A5c a2{};
    std::cout << "test: " << a2.i << std::endl;

    /*
        A5xD > j init, but i is unit 

        j is aggregate init but i users the defined constructor which does nothing.
    */
    A5xd a5xd = {};
    std::cout << "a5dd2" << a5xd.i << a5xd.j <<std::endl;
}

struct A4 {
    int i;
};

// introduces uniform or list initialisation
void Act4(){
    A4 a1;      // default initialization -- as before

    /*
        a2 produces same behaviour as a3 

        > a is initialized in both with an empty braced-init-list
        
        > A4 a = {} is called copy-list-initialisation now.

        1. List init of A4, causes 2
        2. aggregate initialisation as A4 is an aggregate type
        3. The list is empty, all members are intialized by this empty list
            > int i {} leads to value init thus init i to 0
    */

    A4 a2{};    // direct-list-initialization with empty list
    A4 a3 = {}; // copy-list-initialization with empty list
    std::cout << a1.i << " " << a2.i << " " << a3.i << std::endl;

    // what if we use a non empty list though

    
    A4 a4{0}; /* init with 0 */
    A4 a5{{}}; /* init with empty list, value init to 0*/
    A4 a6{a4}; /* copy constructor called passing in a4*/
    std::cout << a1.i << " " << a2.i << " " << a3.i << std::endl;
}

struct A3 {
    /* init i in member init list */
    A3() : i(0) {}
    int i; /* default member init post c++11 'int i = 0' */

};

struct A3b{
    A3b(int i = 0): i(i) {}
    int i;
};

void Act3(){
    /*
        A3 default constructor ensures are value is set to 0
        for default initialisation

        A3b mushes default value which can be provided by user if so required.
    */

    /* calls user defined constructor with default param*/ 
    A3b a1;

    /* calls constructor with value 1*/
    A3b a2(1);

}

/*
    provides same behaviour as
    struct A1 {
        A1(){}
        int i;
    };
*/
struct A12 {
    int i;
};


struct A6 {
    template<typename T>
    A6(std::initializer_list<T>){}
    int i;
};

struct A6b {
    A6b(std::initializer_list<int> l) : i(2) {}
    A6b(int i = 1) : i(i) {}
    int i;
};

// std::intialiser_lists
void Act6(){

    /*
        user provided constructor called each time which does nothing thus i is unintialized
    */

    /* init list with 1 element type int*/
    A6 a1{0};  
    A6 a2{1, 2, 3}; // 2 element init list type int
    A6 a3{"hey", "thanks", "for", "reading!"}; // 4 element init list type const char *
    std::cout << a1.i << a2.i << a3.i << std::endl;

    //A a {} // no type defined thus error, 
    A6 a {std::initializer_list<int>{}}; // must explicitly define an init list.

    {
        /* default init , chooses default constructor with default arguments*/
        A6b a1;

        /* list initialisation with an empty braced-init-list
            
            > A has default constructor with default arguments thus value init occurs
              just calls that constructor , if this constructor was not present
              the initaliser list constructor would be called.
        */
        A6b a2{};

        /*
            direct intilaisation calls constructor with defiend parameter.
        */
        A6b a3(3);

        /*
            list init, overload resolution matches the initializer list constructor first 
        */
        A6b a4 = {5};

        /* cant match against a single int so much call the same initialzer list constructor*/
        A6b a5{4, 3, 2};
        std::cout << a1.i << " "
                << a2.i << " "
                << a3.i << " "
                << a4.i << " "
                << a5.i << std::endl;
    }
}

int main(){

    A12 funcdec(); // function declration

    /* 
        1. default initialized 
        2. calls default constructor
        3. default construct is implicitly defined and does nothing
        4. value i left unintialized.
    */
    A12 a;
    print(" A12 a", a.i);

    /* designated initialiser in C++20, init to 0*/
    A12 a2 = {.i = 0};
    print("A12 a2 = {.i = 0}", a2.i);

   A12 a3 = {0};
   print("A12 a3 = {0}", a3.i);

   A12 a4 = {};
   print("A12 a4 = {}", a4.i);

   /* aggregrate types 
        - simple C style array or struct that looks like simple C struct
        - no access specifiers, no base classes, no user declared constructors
        - no virtual functions
        - get aggregate initialized
   */

   /* aggregate initialisation
        1. each member of the agg is initialized by each element of bracesd list 
           in order
        2. Each member without a corresponding element braced list is 
           value intialized

        > member = class type with user provided constructor > called.
        > member = class type with no user provided constructor > recursive value init
        > member = built int like int > zero init
   */

   /*
    best idea is to explicit initialise stuff.
   */
   Act3();
   Act4();
   Act5();

   /*
    A5b is a class not struct, thus i is private, thus set main as friend.

    This means that A5b is not an aggreate type so we expect its unintialized but it is.

    1. List init of A5b, causes 2
    2. non aggregate > class type with default constructor and empty braced-init-list 
        causes value intilaisation, 3
    3. No user provided constructor , zero intialise the object, 4
    4. Invoke default initialisation if the implicitly defined default constructor is non trivial
       nothing done here.

   */

   A5b a5b {};
   std::cout << a5b.i << std::endl;

   /* 
    same thing as above but using aggregate initlaisation
        1. List initialisation of A , 2
        2. Search for matching constructor
        3. No way to convert 1 to A, compilation failure.
   */

   //A5b a5b2 = {1}; 
}

```

- http://mikelui.io/2019/01/03/seriously-bonkers.html

- start with main then follow the act functions in order looking at each struct associated with it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221019191723548.png)

https://en.cppreference.com/w/c/language/initialization