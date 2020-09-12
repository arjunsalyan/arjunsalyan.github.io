---
layout: post
title: "Golang vs C++: Understanding Strings"
image: golang_1.png
categories: [Programming,GO,C++]
excerpt: "New to Golang and trying to analyse how it is different from the languages you are coming from? Let's compare strings in Golang to strings in C++, and see some under-the-hood stuff."
comments: true
---

> {{page.excerpt}}

While the title says "understanding strings", that is definitely not what we are going to focus on. Instead, we are here to compare what differences we need to be aware of when dealing with strings in C++ vs to that in Golang.

We will be starting with the basics- which you may or may not be aware of. But let's just include everything in our disucssion today. By the end, things will get pretty interesting. Go is interesting in its own ways, and if my plan of writing multiple C++ vs Golang posts works out, the series will make sure that you are able to realise this.

## Difference in definition
First, let's see how to define strings in both the languages, and what happens when we define them.

---
#### In C++:
In C++ (similar to C) a string can be simply defined as an array of character bytes. See the minimal example below, note the terminating null character `'\0'` in the second definition. The under-the-hood stuff in both examples is same- but in the first one, the compiler is smart enough to add the terminating null character by itself.

{% highlight cpp linenos %}
int main() {
    char str1[] = "An array of characters"; // One way to define
    char str2[] = {'A' , 'r', 'r', 'a', 'y', '\0'}; // Another way to define, null character needed
}
{% endhighlight %}

Both `str1` and `str2` are arrays, and that is just all about them.

BUT this is not how we typically work with strings in C++, instead we use the `std::string` class. And that is something to note, **string is a class in C++**. This gives us a lot of power to manipulate strings. Working with them becomes a lot easier as compared to our first way of definition.

The string object stores the different characters of the string as a sequence of character bytes. Thus, we can access all the characters using the indices, just like an array. And also, the size of the string is dynamic, not fixed. Here, take a note of the methods.

{% highlight cpp linenos %}
#include<bits/stdc++.h>

int main() {
    string str = "I am a sequence of character bytes";

    cout << str[0] << endl; // Working the same way like an array

    cout << str.length() << endl; // Class method to get the length of the string

    str.push_back('!'); // Class method to push a character at the end of the string

    cout << str;
}

// OUTPUT:

// I
// 34
// I am a sequence of character bytes!
{% endhighlight %}

#### In Golang:
The string data type in Go works somewhat differently than the string class in C++. First, see the definition:

{% highlight go linenos %}
func main() {
    str := "I am a string"
}
{% endhighlight %}

In Go, the string data type is a read-only slice of bytes. If you don't know what a slice in Go is, think of it like a vector in C++. Thus, we can also access each item using the usual `[]` and proving the index.

{% highlight go linenos %}
func main() {
    str := "I am a string"

    fmt.Println(str[0]) // What do you expect would be printed? 'I'? NO!

    fmt.Println(len(str)) // You got it, prints the length of the string
}

// OUTPUT
// 73
// 13
{% endhighlight %}

Is the fact that `fmt.Println(str[0])` printed "73" and not "I" bothering you? Trust me, neither the compiler, nor the code is incorrect. In Go, the characters are **UTF-8** encoded. When we create a string in Go, it stores the character bytes in the underlying slice. These characters are stored in the form of their ASCII/UTF-8 decimal values. And "73" represents "I" in ASCII/UTF-8.

To summarise for Go:
- A string can unly be defined using the double quotes `"..."` and not single quotes like in python or javascript.
- **A string in Go is immutable (the underlying slice is read-only).**
- Characters are stored in the form of their ASCII/UTF-8 decimal values.

## Code point and rune
So far we have seen how we define a string. We have also seen the underlying data structure and its properties. But one thing which is often overlooked is the fact that not all letters are as simple as a single character byte.

This actually feels totally straighforward as long as we are working with the letters of the English alphabet.

However, results may be unexpected when working with strings containing some special characters, like that of the Chinese language. Our daily experiences force us to think that all characters can be contained in a single character byte, but that's not always true. Some characters do not occupy just one byte. Characters from the Chinese language occupy 3 bytes. Yes, 3.

So, the letters may actually occupy more than one character byte.

{% highlight cpp linenos %}
int main() {
    string str = "日月";

    cout << str.length(); // Guess the output, it is not 2.
}

// OUTPUT:
// 6
{% endhighlight %}

The output is 6. We just learnt that strings are stored as a sequence of bytes and chinese characters take up 3 bytes each. More on your way:

{% highlight cpp linenos %}
int main {
    char s[] = {'H', 'E', '世', '\0'};
}

// warning: multi-character character constant [-Wmultichar]
// char s[] = {'H', 'E', '世', "\0"};
//                       ^~~~~
{% endhighlight %}

Notice the warning, the Chinese character is three times the character byte, and hence cannot be assigned to `char`.

The three bytes that represent this chinese character, can also be termed as a **code point**. In Go, we use UTF-8 encoding and hence for Go, we use the term **rune**. A rune is a type meant to represent a Unicode code point. In rune, we can have a maximum of 4 bytes (32 bit, int32).

This stuff will make more sense when we try to traverse the strings.

## Traversing the strings

First, we will see two ways:

Our good old foor loop, one index to another:

{% highlight cpp linenos %}
int main() {
    string str = "Hello, 世界!";

    int len = str.length();

    for(int i=0; i<len; i++){
        cout << i << " "; // Guess the output, now you can
    }

    cout << endl;

    for(int i=0; i<len; i++){
        cout << str[i] << " "; // Umm, you can sense that something unexpected will happen
    }
}

// OUTPUT:
// 0 1 2 3 4 5 6 7 8 9 10 11 12 13
// H e l l o ,   � � � � � � !
{% endhighlight %}
The first output is simple.

For the second output, we are trying to access each index of the array. However, the Chinese characters are not stored in a single index, and hence the strange output.

This can be solved by using the iterator:

{% highlight cpp linenos %}
int main() {
    string str = "Hello, 世界!";
    
    for(auto x = str.begin(); x != str.end(); x++) {
        cout << *x;
    }

    return 0;
}

// OUTPUT
// Hello, 世界!
{% endhighlight %}
The iterator is not going one index to another, but since it is essentially a pointer, it goes from one code point to another. A

Let's move on to Go now, in Go we will again loop over the string in two ways:
- by using the typical `for` loop like C++
- by ranging over the string (like the iterator in C++)

Let's see:

{% highlight go linenos %}

func main() {
    str := "Hello, 世界!"

    for i:=0; i<len(str); i++ {
        fmt.Printf("%v", str[i]) // This will print the decimal values
    }

    for i:=0; i<len(str); i++ {
        fmt.Printf("%c", str[i]) // This will print the chars, but what happens to the chinese chars?
    }
}

// OUTPUT
// 72101108108111443222818415023114914033
// Hello, ä¸ç!
{% endhighlight %}

I hope the output is making sense now. `%c` tries to convert the ASCII/UTF-8 decimal value to the characters, but when it comes to the single byte of the Chinese characters it cannot convert just a single byte to Chinese. However, the underlying decimal value stored does get converted to some other special characters like `ä¸ç`.

Now, let's see traversing using range.

{% highlight go linenos %}
func main() {
    str := "世界"

    for i, v : range(str) {
        fmt.Println(i)
        fmt.Println(v)
    }
}

// OUTPUT
// 0
// 19990
// 3
// 30028
{% endhighlight %}

It keeps on getting strange, doesn't it? One last thing for today, I promise:

- <ins>__Behaviour of `i`__</ins>: simple. When ranging over a string, `i` does not vary one byte to another byte, instead it varies **one rune to another rune**, recall runes. Hence, first `i = 0`, then it moves to next rune by crossing three bytes and becomes `i = 3`. Also you can see that, `v` will not be a single byte, but a complete rune, or 3 bytes for Chinese characters. These 3 bytes can represent a Chinese character.
- <ins>__Behaviour of `v`__</ins>: We just learnt that `v` is a set of three bytes, it contains the entire Chinese character now. Here, it is correct to expect that the Unicode decimal value of the Chinese characters get printed. For that we need to go to [Unicode Table](https://www.tamasoft.co.jp/en/general-info/unicode-decimal.html) and search for the character `世`. As we anticipated, we find its Unicode decimal value to be **19990**. Feeling good about it?

---

I hope you could get atleast something out of this article. If you are completely new to these concepts, you might want to go out and try some experiments yourself, that is the best way to learn.

If you are not new and understand things pretty well, let's build a conversation over things I missed or explained in not-so-good/ incorrect manner.

I hope to write more Golang vs C++ posts.