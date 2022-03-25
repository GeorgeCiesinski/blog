---
layout: single
title: "Binary Files"
date: 2022-03-24 23:00:00 -0500
categories: Programming
comments: true
typora-root-url: ..
---

# Binary Files

A few days ago, I found a question on Reddit where a user was asking for someone to give a basic explanation of what binary files are. When I was writing my explanation, I ended up learning some new things and decided I should put my answer up in my blog since I had fun writing it. Keep in mind this answer is for a beginner, and I am also not an expert in Binary or Machine Code, so a lot of this information is simplified and summarized. I hope you enjoy this post!

# What are Binary files

Binary files are just computer files that consist of 1's and 0's. These individual 1's and 0's are called "bits" and 8 of them form a [byte](https://en.wikipedia.org/wiki/Byte). A byte is usually the smallest addressable unit of memory in many computer architectures and looks like `10110000`.

Bytes of data can represent different types of information. For example, the letter A is represented by `01000001`, but this byte ALSO is the binary value of 65. Usually, binary files also includes instructions telling the computer whether it should read the binary as a letter, a number, or as some other kind of data.

## What Binary files are used for

Binary files are a way for a computer to understand many kinds of data. For example:

* Images
* Audio
* Numerical and text data
* Code (including logic)

First of all, binary files are specifically designed for computers to read and are not meant to be human readable. This is useful for computers because computers use processors to interpret the data, and these processors are made from silicon components called [transistors](https://en.wikipedia.org/wiki/Transistor) which can be turned on (1) or off (0). There are tens of billions of these transistors inside of a processor that carry out logical operations by rapidly turning on and off (I'll talk about this later).

## How does a Computer make sense of Binary data?

Lets consider an image file that is stored in binary. Such a file would basically be a large file containing 1's and 0's. To you, it would look like a loooooong string of 0's and 1's. An image viewer on your computer would see the file as chunks of 1's and 0's, with each chunk being a certain pattern the image viewer was programmed to recognize. One of these patterns might represent the first pixel of an image on the top left hand corner of a picture. The next few patterns might represent this pixel's red, green, and blue values. Other kinds of files are basically the same idea. Your computer is able to translate binary files as chunks of binary patterns which it converts into something you can see, hear, or read, and it does this using programs written in higher level languages like Python.

**Quick note:** Low level languages are languages that directly controls a computer's processor but is difficult for humans to read and write. High level languages are actually written using lower level languages and are way easier for humans to actually use. Python is a high level language that was written using the lower level language called [C](https://en.wikipedia.org/wiki/C_(programming_language)), and this language was in turn written using an even lower level language known as [assembly](https://en.wikipedia.org/wiki/Assembly_language) or machine code. Thanks to this, we can write a program in human understandable code and the compiler or interpreter changes this into machine code for the processor to actually execute.

# What is Machine Code

Now lets talk about assembly or [machine code](https://en.wikipedia.org/wiki/Machine_code). This topic can get extremely complicated so forgive this brief explanation. Machine code consists of machine language instructions consisting of 1's and 0's. These instructions tell the processor to store or load data from memory, jump to a different memory register, or carry out arithmetic operations (math basically) using other data. The billions of transistors in a processor carry these instructions out millions or billions of instructions per second. The instructions look sort of like `10110000 01100001` which may tell the processor to move a byte sized piece of data into a specific memory register.

**Quick note 2:** A memory register is just an 8 bit large piece of memory that data can be stored in by a processor. Imagine adding 2+5 in your head. Your brain is remembering the 2 and the 5 while you are adding them up. Processors are kind of similar in that they store data as they calculate it.

# What are Logic Operations

Besides moving data around and executing math calculations, processors also execute [bitwise operations](https://en.wikipedia.org/wiki/Bitwise_operation#Bitwise_operators). These are basically machine executable [logic operations](https://whatis.techtarget.com/definition/logic-gate-AND-OR-XOR-NOT-NAND-NOR-and-XNOR) such as AND, OR, NOT, XOR, etc.

Each of these operators require one or more inputs and generate a result.

* The **AND** gate gives a result of 1 only when both inputs are 1, otherwise the result is 0.
* The **OR** gate gives a result of 1 when either or both inputs are 1, and 0 if both inputs are 0.
* The **XOR** or (eXclusive OR) gate gives a result of 1 if either input is 1, but a result of 0 if both are 1, or if both are 0
* The **NOT** gate (also known as an inverter) only takes in one input and gives a result opposite of the input (if 1 then 0, if 0 then 1)

Besides these, there are more specialized gates that perform similar logical operations involving inputs and outputs (results). Performing a larger number of these logical operations can be used to perform math, or conditional logic.

## Logic Gates

These logic operations are digital versions of [logic gates](https://en.wikipedia.org/wiki/Logic_gate#History_and_development) which were proposed as early as the 1880's and were used way before computers to carry out logical operations (for example, switching circuits on/off, or turning machines off automatically if a safety was tripped, etc). Back then, we used large vacuum tubes and bulky electrical components to build logic gates. Today, we use transistors too small to see with the naked eye that can carry out more logical operations than an entire city full of logic gates could 100 years ago. These were basically very simple computing devices that are in a way ancestors of the modern computer.

## AND: an example of a Logic Gate

If you wanted to make a nuclear launch button where two people both had to turn a separate key for the missile to launch, you would use the AND operator: `if a_key AND b_key THEN launch_missile`.

The actual logical operation would take in the values A and B and then calculate the result. The result would always be 0 except when A and B are both 1:

```
+---+---+--------+
| A | B | Result |
+---+---+--------+
| 0 | 0 | 0      |
+---+---+--------+
| 1 | 0 | 0      |
+---+---+--------+
| 0 | 1 | 0      |
+---+---+--------+
| 1 | 1 | 1      |
+---+---+--------+
```

All of the logic gates can be plotted onto a similar chart where there are one or more inputs and a result.

## Logic operators and the bigger picture

The processor sees these logical operators in the machine code as specific patterns of 1's and 0's and the transistors execute the operation to find the value. On it's own, a single logical operation might flip a 0 to a 1, but when you have tens of billions of transistors carrying out billions of these operations per second, you can combine different operations to add two numbers together, or to take the product of two values and copy it into a specific memory register. A few billion operations later and you might be rendering a new image 60 times a second which the human eye sees as a video game character jumping over an enemy.

# Conclusion

So anyways, to summarize it, binary files are just computer files that are made up of 1's and 0's and the computer can easily recognize patterns of these 1's and 0's as instructions. Once it has the instructions, it executes them using billions of transistors in the processor, and stores the result in the memory. It can then repeat the instructions using the stored result until it translates those 1's and 0's into something a human understands - like pictures, sound, text, or anything else you can imagine using a computer for.
