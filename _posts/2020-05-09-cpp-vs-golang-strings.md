---
layout: post
title: "Golang vs C++: Understanding Strings"
image: golang_1.png
categories: [Programming,GO,C++]
excerpt: "New to Golang and trying to analyse how it is different from the languages you are coming from? Let's compare strings in Golang to strings in C++, and see some under the hood things."
comments: true
---

> {{page.excerpt}}

While the title says "understanding strings", yet I know you are not here to see what a string is, instead what differences you need to be aware of when dealing with strings in C++ vs to that in Golang. Hmm, so the context is clear, let's begin then.

We will start with very basic, but by the end, things will get pretty interesting. Go is peculiar in many ways, if my plan of writing multiple C++ vs Golang series works out, the series will make sure that you are able to realise this.

### Difference in definition
---
<br>
#### In C++:
Similar to C, in C++ a string can be simply defined as an array of character bytes. Let's see the simple example, note the terminating null character `'\0'` at the last index in the second definition. However, in the first definition, the compiler automatically adds the terminating null character for us.

```c
char str[] = "An array of characters"; // One way to define
char str2[] = {'A' , 'r', 'r', 'a', 'y', '\0'}; // Another way to define, null character needed
```
<br>
Now, it's just an array, you can traverse it whatever way you want.

But this is not how we typically work with strings in C++, instead we use the `std::string` class. Well, that is something to note, string is a class in C++. The string object stores the different characters of the string as a sequence of character bytes. You can also access the characters by using the index, just like an array, neat. And yeah, the size of the string is dynamic and not fixed like that of the array.

The string class also provides you multitude of methods to make working with strings pretty easy.

```c
#include<bits/stdc++.h>

int main() {
    string str = "I am a sequence of character bytes";

    cout << str[0] << endl;

    cout << str.length() << endl; // Class method to get the length of the string

    str.push_back('!'); // Class method to push a character at the end of the string

    cout << str;
}

// OUTPUT:

// I
// 34
// I am a sequence of character bytes!
```
<br>
#### In Golang:
If you are coming from C++, I am telling you things are going to get very interesting here. The string data type in Go, is quite different from what you are used to in C++.

In Go, a string data type comprises of two things
- A read-only slice of bytes (If you don't know what a slice in Go is, think of it like a vector in C++)
- length `len`

And that is it. Now, the characters in Go are **UTF-8** encoded. So, when you create a string in Go, it stores the character bytes in the underlying slice. What is interesting is, the characters are stored in the form of their ASCII/UTF-8 decimal values. Let's see it in action.

```go
str := "I am a string"

fmt.Println(str[0]) // What do you expect would be printed? 'I'? NO!

fmt.Println(len(str)) // You got it, prints the length

// OUTPUT
// 73
// 13
```
<br>
Wait, 73? Yes, 73. Remember, we discussed that the characters are stored as their ASCII/UTF-8 decimal values. Some points to note about strings in Go:

- A string can unly be define using the double quotes `"..."` and not single quotes like in python or javascript.
- **A string in Go is immutable, you cannot assign values to indexes.**
- Characters are stored in the form of their ASCII/UTF-8 decimal values.

<br>
### Traversing the strings (code point and rune)
---
<br>
It is straightword in C++:, let's see a simple example.
```c
string str = "Hello, World!";

for(int i=0; i<str.length(); i++){
    cout << str[i];     // Accessing each byte
}

// OUTPUT:
// Hello, World!
```
<br>
So far so good, as long as we work with the letters of the English alphabet, everything just works smooth. However, results might surprise you when your strings start containing some special characters, like that of the Chinese language. Our daily experiences force us to think that all characters can be contained in a single character byte, but that's not always true. Some characters do not occupy just one byte. Characters from the Chinese language occupy 3 bytes. Yes, 3.

```c

string str = "日月";

cout << str.length(); // Guess the output, it is not 2.

// OUTPUT:
// 6
```
<br>
The output is 6, that's easy to tell, why it is 6. We just learnt that strings are stores as a sequence of bytes and chinese characters take up 3 bytes each.

I guess this will be more clear with the following example:

```c
char s[] = {'H', 'E', '世', '\0'};

// warning: multi-character character constant [-Wmultichar]
// char s[] = {'H', 'E', '世', "\0"};
//                       ^~~~~
```

Noticed the warning, the Chinese character is three times the character byte, and hence cannot be assigned to `char`.

The three bytes that represent this chinese character, can also be termed as a code point. In Go, we use UTF-8 encoding and hence there we use the term rune. A rune is a type meant to represent a Unicode code point. In rune, we can have a maximum of 4 bytes (32 bit, int32).

Fine, let's try to traverse over strings in both C++ and Go.

```c
string str = "Hello, 世界!";

int len = str.length();

for(int i=0; i<len; i++){
    cout << i << " "; // Guess the output, trust me now you can.
}

cout << endl;

for(int i=0; i<len; i++){
    cout << str[i] << " "; // This will be tricky
}

// OUTPUT:
// 0 1 2 3 4 5 6 7 8 9 10 11 12 13
// H e l l o ,   � � � � � � !
```
<br>
I hope you could predict the first output. But the second output in which try to print each character is tricky.
Recall, that in a string in c++ we can access each single character byte, but for the chinese letters, how does your compiler convert that single byte to a character, it can't. Hence we get those strange unrecognizable letters, these can vary by the way, depending on how and where you run your program.

Let's move to Go now, in Go we can loop over the string in two ways, by using the typical for loop from C/C++ or by ranging over the string, let's see/

```go
str := "Hello, 世界!"

for i:=0; i<len(str); i++ {
    fmt.Printf("%v", str[i]) // This will print the decimal values
}

for i:=0; i<len(str); i++ {
    fmt.Printf("%c", str[i]) // This will print the chars, but what happens to the chinese chars?
}

// OUTPUT
// 72101108108111443222818415023114914033
// Hello, ä¸ç!
```
<br>
I hope the output is making sense now. `%c` tries to convert the ASCII/UTF-8 decimal value to the characters, but when it comes to the single byte of the Chinese characters, it cannot convert them to Chinese by access to just a single byte, however the underlying decimal value stored does get converted to some other special characters like `ä¸ç`. This can be explained and cannot be, because if you try to copy paste the characters `ä¸ç`, things may change surprisingly. A simple way to understand this is just think that some decimal value was stored at each byte (three together would make up chinese, a single would not), compiler tried to convert those decimal values to characters but failed.

Now, let's see traversing using range.

```go
str := "世界"

for i, v : range(str) {
    fmt.Println(i)
    fmt.Println(v)
}

// OUTPUT
// 0
// 19990
// 3
// 30028
```
<br>

It keeps on getting strange, isn't it? Let us try to undertand it now.

- Behaviour of `i`: simple. When ranging over a string i does not vary one byte to another byte, instead it varies **one rune to another rune**, recall runes. Hence, first `i = 0`, then it moves to next rune by crossing three bytes and becomes `i = 3`. Also you can see that, `v` will not be a single byte, but a complete rune, or 3 bytes for Chinese characters. These 3 bytes can represent a Chinese character.
- Behaviour of `v`: We just learnt that `v` is a set of three bytes, it contains the entire Chinese character now. So what should be printed is the Unicode decimal value of that Chinese character. And if you go to [Unicode Table](https://www.tamasoft.co.jp/en/general-info/unicode-decimal.html) and search for the character `世`, you will be find its Unicode decimal value to be **19990**. Neat.

---

I hope you could get atleast something out of this article. If you are completely new to these concepts, you might want to go out and try some experiments yourself, that is the best way to learn.

If you are not new and understand things pretty well, let's build a conversation over things I missed or explained in not-so-good/ incorrect manner.

I hope to write more Golang vs C++ posts.