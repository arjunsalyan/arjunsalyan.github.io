---
layout: post
title: "Golang vs Object Oriented Languages"
image: golang_1.png
categories: ["GO", "OOPS"]
excerpt: "A comprehensive discussion on the basics of OOPS and how differently they are implemented in Golang."
comments: true
---

> In this post we will discuss the basics of Object Oriented Programming and how they are implemented in Golang.

As everybody knows, Go is peculiar. In my opinion it is caused by the ambition to be transparent, simple and fast. In a previous post, we compared [how differently Golang works with string when compared to C++](/cpp-vs-golang-strings/).

Today let us take a deeper look into how the various ideas of OOP are implemented in Golang. There is a pre-requisite before we move to comparison- and that is a basic understanding of OOP. We will first see that.

## OOP: Formal Definition

<ins>From Wikipedia</ins>: *"Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which can contain data, in the form of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods)."*

## OOP: A Simpler Definition

If you could understand the definition from Wikipedia, well and good. But if you are new to this- let us see it in a different view. Everything in the world can be classified into classes- for example, you and me are objects of the class "human". For a better understanding let us focus our discussion to the human class only. Every human has two types of features- one is state (fields) and another is behaviour (methods). Similarly an Object in the OOP paradigm has attributes and methods.

##### 1. Fields (attributes)

You and me both are humans, but we are different. What makes us different? Height, weight, eye-color etc. So, human is a class and once we set different values for the attributes(height, weight, eye-color,...an infinite list in case of humans...) of this class- it becomes an object. Hence, you and are me are different objects from the class human because we have different values of the attributes that define a human.

In other words, these attributes define the state of an object. Imagine a String class, what can be the attributes for it? Umm, pointer to the charactery array, length of the character array etc. If I have two different String objects they will have different values for the pointer and the length.

*A class becomes an object once the values for the fields are initialised.*

##### 2. Methods (behaviour)

We are now clear that you and me are different objects from the same class human. But we perform a lot of tasks that are similar- breathing, eating, blinking the eyes etc. These behaviours can change the state of a human. For example, if I eat a lot my weight may increase. Hence, behaviours or methods can change states of a human.

Coming back to the String class example, what are the methods that come to your mind? Appening to the string, erasing characters from the string, returning length of the string etc. Some of these will also change the values of the attributes, for example length will change if we add insert more characters to the string.

*Methods may change the state of the objects*.

<hr>
<br>
Let us move towards the technical side now, in object oriented programming, everything is considered as an object. As we have seen, an object has two types of features- attributes and methods. `Java` is a very popular example OO language. `C++` does not enforce OOP, however, you can achieve both procedural and object oriented proagramming in `C++`.

Those coming from C++ will find it strange that we have `Integer` class in java (everything is wrapped in an object, even `int`). The Integer class wraps a value of the primitive type `int` in an object. An object of type Integer contains a single field whose type is int. 

**Let us write a simple class in `C++`. I am choosing C++ only for the definition of classes and objects, because most of the readers of my blog prefer C++, but later on in the tutorial, we will consider Java.**

{% highlight c linenos %}

class Human {
    public:

        // The attributes
        int weight;
        int height;
        int age;

        // The methods
        int breath() {
            printf("A human of age %d is breathing", age);
        }

        int HappyBirthday() {
            this->age += 1;  // this is a pointer to the current object
        }
};

int main() {
    Human me; // A human is initialised

    // Let us set the attributes for "me"
    me.height = 175;
    me.weight = 70;
    me.age = 22;

    // I get one year older
    me.HappyBirthday();
    std::cout << me.age; // Shall print 23 which is 22 + 1
}

// OUTPUT:
> 23

{% endhighlight %}

#### An object(?) in Go

We have have our first point of comparison. How do we define objects in Go? In Go, data and functions are kept separate. There are no classes in Go, just `structs` which can only store data and do not have any methods. But we can achieve methods using receivers (notice the `()` block right after `func`).

{% highlight go linenos %}
type Human struct {
    Height  int
    Weight  int
    Age     int
}

func (h *Human) HappyBirthday() {
    h.Age += 1
}

func main() {
    me := Human{ Height: 175, Weight: 70, Age: 22 }
    me.HappyBirthday()
    fmt.Println(me.Age)
}

// OUTPUT
> 23
{% endhighlight %}
## Moving forward to Real Business

So far, we were more concerned about what OOP is and less about the comparison with Go. Now that the pre-requisite is met, we can continue the discussion.

There are three phenomenon in OOP,

1. Encapsulation
2. Polymorphism
3. Inheritance

Our real comparison will be based on how we deal with these three things in Java vs to that in Go.

## 1. Encapsulation

When we defined our "Human" class in C++ above, did you notice that we had the term `public` before everything?

Encapsulation, in short, is controlling access to the information of an object. For example, a human might not be want to expose his/ her age to other objects. Similar thing can be achieved while making a class in programs. We control this access using access modifiers. These are:

1. <ins>**public**</ins>: Methods and attributes that use the public modifier can be accessed within your current class and by all other classes.

2. <ins>**protected**</ins>: Attributes and methods with the access modifier `protected` can be accessed within the class, by all other classes within the same package, and by all subclasses within the same or other packages. We will see what subclasses are later in `Inheritance`.

3. <ins>**private**</ins>: This is the most restrictive and commonly used access modifier. A `private` attribute or method can only be accessed within the same class. Subclasses or any other classes within the same or a different package can’t access a private attribute or method.

4. <ins>**No modifier**</ins>: What if we do not provide any modifier at all. In such cases a language-specific default is used. In Java, this defaults to package-private or access within the same class and by all classes of the same package. I C++, the default is `private`.


So, I am assuming you have understood how we achieve `encapsulation` in Java or C++, just by adding one of the modifiers before the attributes/methods. I will still write the Java code for you below, also refer the C++ code above.

##### Encapsulation in Java

{% highlight java linenos %}
//A Java class which is a fully encapsulated class.  
//It has a private data member and getter and setter methods.  
package com.encapsulation; 

public class Student{  
    //private data member  
    private String name;
    private String college = "IIT BHU";  

    // method to get a student's name 
    public String getName(){  
        return name;  
    }  

    // method to set a student's name 
    public void setName(String name){  
    this.name=name  
    }  
} 
{% endhighlight %}
It makes complete sense to have a small discussion on the above written Java Code. There are two attributes: `name` and `college` and both are private. This means that we cannot modify or access them directly. We also have two methods, `getName()` and `setName()`, both are public and we can access them from anywhere.

So, to set a student's name we can call `student.setName("Arjun Salyan")` and to get a student's name we can call `student.getName()`. This will work because private attributes can be accessed by the methods of the same class.

Notice that, we cannot modify `college` of a student because the attribute is private and there is no method that will help us modify it. I hope you can now think of the use cases and really appreciate encapsulation.

##### Encapsulation(?) in Go

Hmm, so can we have something similar to encapsulation in Go? Yes and No. First of all, there are no access modifiers in Go. All we have are exported and unexported fields. Any method or field whose name start with an uppercase letter are considered exported in Go, while lowercase are unexported.

An exported field or function can be accessed "directly" from other packages, while unexported cannot be. And that is it. I will write two structs below, first one has exported fields and other does not.

**NOTE**: Unexported fields cannot be accessed directly from other packages does not mean we cannot access them. It is just that we cannot access them directly, I hope you can now figure out on your own what I mean by this!

{% highlight go linenos %}
package data

import (
    "fmt"
)

type ExportedFields struct {
    Field1 string
    Field2 string
}

type unexportedFields struct {
    field1 string
    field1 string
}

func(f *nexportedFields) NewUnexportedFields(val1, val2 string) *unexportedFields {
    return &unexportedFields {
        field1: val1,
        field2: val2
    }
}
{% endhighlight %}

Very Important that we discuss this code. There are two structs, `ExportedFields` and `unexportedFields`. `ExportedFields` starts with uppercase and hence it can be accessed from other packages- initialisation and modification both can be done from anywhere.

But `unexportedFields` is different, neither the package nor the internal fields have been exported. So, let's say we want to create an object of type `unexportedFields` from any other package, how do we continue? Simple, write a method that returns a created object and export the method.

In the above example, `NewUnexportedFields` is a function that is exported and can be access from anywhere. We use this to create new objects of the type `unexportedFields`. See below

{% highlight go linenos %}
package main

import (
    "data"
)

func main() {
    myObject := data.NewUnexportedFields("value_for_field_1", "value_for_field_2")
}
{% endhighlight %}
<br>

That should be enough for encapsulation. We have learnt encapsulation and how to achieve something similar in Go.

## 2. Polymorphism


***Story under construction***