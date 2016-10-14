# ReadOnly

[![Build Status](https://travis-ci.org/illescasDaniel/ReadOnly.svg?branch=master)](https://travis-ci.org/illescasDaniel/ReadOnly)
[![Version](https://img.shields.io/badge/release-v1.2.1-green.svg)](https://github.com/illescasDaniel/ReadOnly/releases)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/illescasDaniel/ReadOnly/blob/master/LICENCE)  

Manage read only members in C++ classes.

How to make it work
------
Add your friend class in the ReadOnly class.  
```C++
class ReadOnly {

	/** Friend classes **/
	friend class Foo;

	/* ... */
};
```

Basic syntax
------
```C++
#include "ReadOnly.hpp"

class Foo {

	static bool setName(const string& value) {
		return (value.length() <= 20);
	}

public:

	// Initialize variable specifying a setter function
	ReadOnly<string> name {Foo::setName}; 

	// This variable has a default value of 10, and can't be changed externally 
	// You could change its value internally accessing its value property (number.value) 
	ReadOnly<int> number = 10; 

	Foo() {}

	// This will assign the new value only if the specific setter function returns true
	Foo(const string& name) { 
		this->name = name; 
	}
};

Foo object("John");

cout << object.name << ' ' << object.number << endl; // Output: John 10

object.name = "Daniel"; // OK  
object.name = "aaaaaaaaaaaaaaaaaaaab" // ERROR

string objectName = object.name;  
string myName = "Daniel Illecas";  
object.name = myName;  

object.number = 20; // ERROR, variable is read only and doesn't have a setter

```

Motivation
--------
In OOP when you use classes you have attributes and methods. 
The problem is that on some languages, as C++, if you want someone to access your variables, they
have to use (most of the times) getters, so instead of something like: 

```C++
Human daniel;  
cout << daniel.age << endl;  
```  

You would have to do this: 

```C++
Human daniel;  
cout << daniel.getAge() << endl; 
```

Instead of accessing the attribute of the object, its property, you need to call a function, **which doesn't make sense in OOP**.  

With the ReadOnly class you can declare a variable which value will only change (externally) if a setter function is specified and if the value satisfy the setter.  

**Note**: by default constructors are private, hence you can only declare variables inside a friend class.
