# Containers
***
There are two types of containers.

- Sequence:  
	● Containers that can be accessed  sequentially  
	● Anything with an inherent order  goes here!
- Associative  
	● Containers that don’t necessarily have a sequential order  
	● More easily searched  
	● Maps and sets go here!

## Vector
### Initialization
Notice the slight difference
```cpp
std::vector<int> vec1(3,5);
//makes {5,5,5}
std::vector<int> vec2{3,5};
//makes {3,5}
```
