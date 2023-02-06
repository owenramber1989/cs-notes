# List initialization
1. int a = 0;
2. int a = {0};
3. int a{0}
4. int a(0)
Using the curly braces to initialize is the so-called ***list initialization***.  The compiler will not let us list initialize variables of built-in type if the initializer might lead to the loss of information. i.e. You can not do sth like int a = {foo/\*which is long double type\*/}.
# Scope
Since the global scope does not have a name, so when the scope operator has an empty left-hand side, it is a request to fetch the name on the right-hand side from the global scope.
Pay special attention to the 3rd one. **::reused**.
```cpp
#include <iostream>
// Program for illustration purposes only: It is bad style for a function
// to use a global variable and also define a local variable with the same name
int reused = 42;
 // reused has global scope
int main()
{
int unique = 0; // unique has block scope
// output #1: uses global reused; prints 42 0
std::cout << reused << " " << unique << std::endl;
int reused = 0; // new, local object named reused hides global reused
// output #2: uses local reused; prints 0 0
std::cout << reused << " " << unique << std::endl;
// output #3: explicitly requests the global reused; prints 42 0
std::cout << ::reused << " " << unique << std::endl;
return 0;
}
```
# reference
There are two kinds of refs - the lvalue ref and the rvalue ref.
|lvalue ref|rvalue ref|
|---|---|
|use &|use &&|
|addressable|&#10008|
|assignable|&#10008|

Generally lvalue can be modified while rvalue not(in the context of basic types). User-defined rvalue ref can be modified by member function.
# types & structs
The huge diffrences: statically typed or dynamically typed.
**statically** : everything with a name is given a type before runtime
dynamically: everything with a name is given a type ***at*** runtime based on the thing's current value
***Runtime*** is the period when program is executing commands after compilation( if compiled).
## size_t type
[size_t](https://en.cppreference.com/w/cpp/types/size_t) type is a base unsigned integer type of C and C++ language. It is the type of the result returned by sizeof operator. The type's size is chosen so that it can store the maximum size of a theoretically possible array of any type. On a 32-bit system size_t will take 32 bits, on a 64-bit one 64 bits. In other words, a variable of size_t type can safely store a pointer. The exception is pointers to class functions, but this is a special case. Although size_t can store a pointer, it is better to use another unsigned integer type [uintptr_t](https://pvs-studio.com/en/blog/terms/0050/) for that purpose (its name reflects its capability). The types size_t and uintptr_t are synonyms. size_t type is usually used for loop counters, array indexing, and address arithmetic.

The maximum possible value of size_t type is constant SIZE_MAX.
```cpp
void shift(vector<std::pair<int, int>>& nums) {  
for (size_t i = 0; i < nums.size(); ++i) {  
	auto& [num1, num2] = nums[i];  
	num1++;  
	num2++;  
	}  
}  
47
```
Tips: What would happen if the & in line3 is removed?
# auto
The key word **auto** does **not** mean that the variable does not have a type. It means that the type is ***deduced*** by the compiler.Feel free to use auto to reduce long type names but do not overuse it.
## structured binding 
```cpp
auto p =  
std::make_pair(“s”, 5);  
string a = p.first;  
int b = p.second;
```
 can be reduced to
 ```cpp
auto p =  std::make_pair(“s”, 5);  
auto [a, b] = p;  
// a is string, b is int  
// auto [a, b] = std::make_pair(...);
```
