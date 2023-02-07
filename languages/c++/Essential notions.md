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
|lvalue|rvalue|
|---|---|
|use &|use &&|
|addressable|&#10008|
|assignable|&#10008|
|can appear on the left or right of an =|only right|
|have a name|do not|
|not temporary|temporary|

So there's a common misuse of the reference. If the parameter of a function is a ref, you are not allowed to pass an r-value argument **in most cases**.
***
	DO NOT declare not-const reference to const variables.
***
- If we need to alias the variable to modify it, we can  
use references  
- If we don’t need to modify the variable, but it’s a big  
variable (e.g. std::vector), we can use const references  
***

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
# Stream
***
Stream is an abstraction for  input/output. Streams  convert between data and  the string representation  of data.  
e.g. cout
***Except for primitive types and most from the STL, you will have to write the << operator urself.***
	The type of `std::cout`  is `std::ostream`
## File stream
```cpp
std::ofstream out(“out.txt”);  
// out is now an ofstream that outputs to  out.txt  
out << 5 << std::endl; // out.txt contains 5 
```
## Input stream 
### when things go wrong
```cpp
string str;  
int x;  
string otherStr;  
std::cin >> str >> x >> otherStr;  
//what happens if input is blah blah blah?  
std::cout << str << x << otherStr;
```
First of all, there would not be a crush. X stores 0 to indicate a fail. And there would be stored ***nothing in otherStr***. Cuz once an error is detected, the input stream's fail bit is set, and it will no longer accept input.
## std::getline()
```cpp
istream& getline(istream& is, string& str, char delim);
//The function store output in str.
```
### How it works:  
- Clears contents in str  
- Extracts chars from is and stores them in str until:  
	- End of file reached, sets EOF bit (checked using is.eof())  
	- Next char in is is delim, extracts but does not store delim  
	- str out of space, sets FAIL bit (checked using is.fail())  
- If no chars extracted for any reason, FAIL bit set  
In contrast:  
● “>>” only reads until it hits  whitespace (so can’t read a  sentence in one go)  
● BUT “>>” can convert data to  built-in types (like ints) while  getline can only produce strings.
● AND “>>” only stops reading at  predefined whitespace while  getline can stop reading at any  
delimiter you define.


 