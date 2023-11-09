---

---

# Printing Floating Point Numbers

## cout

 The <kbd>cout</kbd> command is considered to be *non-specific* because you can use the same syntax for all of your printing needs. However, for printing certain numbers, it is not always clear if what's printed is an <kbd>int</kbd> or a <kbd>double</kbd> sometimes.

 Even though you are printing a <kbd>double</kbd> of <kbd>1.0</kbd>, the system will disregard the decimal and the trailing zero when <kbd>cout</kbd> is used. There is another print command called <kbd>printf()</kbd> that also prints text to the console.



## printf()

<kbd>printf()</kbd> originates from the C language and, unlike thet <kbd>cout</kbd> command, it is considered to be *specific*. This means that you must specify what *type of data* you want to print before you can use the command successfully.

---

# Incrementing Variables

## Incrementing Variables

**Incrementing** a variable means to increase the value of a variable by a set amount. The most common incrementation you will see is when a variable increments itself by the value of 1.



## The <kbd>++</kbd> and <kbd>+=</kbd> Operators

Since implementing is such a common task for programmers, many programming languages have developed a shorthand for <kbd>a = a + 1</kbd>. The result is <kbd>a++</kbd> which does the same thing as <kbd>a = a + 1</kbd>.

---



# String Concatenation

## String Concatenation

**String concatenation** is the act of combining two strings together. This is done with the <kbd>+</kbd> operator.

---



# Subtraction

## The <kbd>--</kbd> and <kbd>-=</kbd> Operators

 **Decremeting** is the opposite of incrementing. Just like you can increment with <kbd>++</kbd>, you can decrementing using <kbd>--</kbd>.

 Like <kbd>+=</kbd>, there is a shorthand for decrementing a variable, <kbd>-=</kbd>. For example, if you want to decrement the variable <kbd>a</kbd> by <kbd>2</kbd> instead of <kbd>1</kbd>, replace <kbd>a--</kbd> with <kbd>a-=2</kbd>.



# Division

**Division** is C++ is done with the <kbd>/</kbd> operator.

> Division by zero is *undefined* in mathematics. In C++, dividing by an **integer** of <kbd>0</kbd> results in an error message. However, dividing by a **double** of <kbd>0.0</kbd> results in <kbd>inf</kbd> which is short for *infinity*.



## Integer Division

 Normally, you use <kbd>double</kbd> in C++ division since the result usually involves decimals. If you use integers, the division operator returns an <kbd>int</kbd>. This "integer division" does not round up, nor round down. It removes the decimal value from the answer.

---

# Order of Operations

C++ uses the **PEMDAS** method for determining order of operations.

![image](../assets/img/Arithmetic_operators/media%2F1%2Fd324ba7d6df7e53481310f3f7b3da2e3-78fab70a-7183-4b5f-b7ef-a1c6b1873291.webp)

 By default, there are no operators for **exponents** and **square roots**. Instead, functions such `pow( , )` and `sqrt()` are used to calculate powers and square roots respectively. In order to use these functions, they must be imported by including `#include <cmath>` at top of the program header. For exponents, the *base* number goes before the `,` in `pow( , )` and the *exponent* goes after the `,`. For example, `pow(4, 2)` calculates $4^2$ and `pow(4, 0.5)` calculates $4^{0.5}$ or $4^{1/2}$. For square roots, the number goes inside the `()` in `sqrt()`. An example is `sqrt(4)` which calculates $\sqrt{4}$.

---

# Type Casting

** Type casting**(or type conversion) is when you change the data type of a variable. You can use <kbd>(double)</kbd>,<kbd>(int)</kbd>, and <kbd>(bool)</kbd> to cast any double, integer, or boolean between each other. Note that casting an intger of <kbd>0</kbd> or a double of <kbd>0.0</kbd> to a boolean will result in <kbd>false</kbd>. Any other integer or double values will result in <kbd>true</kbd>.

---

# Data Type Compatibility

 In C++, you can add a combination of <kbd>int</kbd>s, <kbd>double</kbd>s, and <kbd>bool</kbd>s together. Remeber that a boolean is either <kbd>1</kbd> if it's <kbd>true</kbd> or <kbd>0</kbd> if it's <kbd>false</kbd>. 

 You can convert the string to an integer to fix the problem by using <kbd>stoi()</kbd>. <kbd>stoi</kbd> acts as a **function** to convert a string into an integer. The string or string variable goes into the <kbd>()</kbd> to be converted. 