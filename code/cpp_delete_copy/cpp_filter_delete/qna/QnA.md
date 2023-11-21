# Advanced C++ Interview Questions and Answers

##### What are friend functions?

```c++
#include <iostream>


class Base {

	int x {};

public:

	Base() {}
	Base(int x) : x{x} {}

	/*  this function now has access to the private members of the class
		this does not break encapsulation as we enforce it as a rule of the class
	*/
	friend void fun(Base&);

};

void fun(Base& obj) {
	std::cout << obj.x << std::endl;
	obj.x = 20;
	std::cout << obj.x << std::endl;
}

int main() {
	
	Base b(10);
	fun(b);
	return 0;
}
```

###### Use case / Other Notes

- Admin class that must have access to all the functions of some employee.
- Friend functions in inheritance hierarchies access what is public / protected in that specific point of the tree.
- Often used in testing to allow some testing function to be a friend of the class.