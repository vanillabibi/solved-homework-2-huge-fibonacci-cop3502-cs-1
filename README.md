Download Link: https://assignmentchef.com/product/solved-homework-2-huge-fibonacci-cop3502-cs-1
<br>



In this programming assignment, you will implement a Fibonacci function that avoids repetitive computation by computing the sequence linearly from the bottom up:<em><sub>F</sub></em>(0) through <em><sub>F</sub></em>(n). You will also overcome the limitations of C’s 32-bit integers by storing very large integers in arrays of individual digits.

By completing this assignment, you will gain experience crafting algorithms of moderate complexity, develop a deeper understanding of integer type limitations, become acquainted with unsigned integers, and reinforce your understanding of dynamic memory management in C. In the end, you will have a very fast and awesome program for computing huge Fibonacci numbers.

<strong>Attachments</strong>

Fibonacci.h, fib-main{01-05}.c, fib-output{01-05}.txt

<strong>Deliverables</strong>

Fibonacci.c

(Note: Capitalization of your filename matters!)

<h1>1           Overview</h1>

<h2>1.1          Computational Considerations for Recursive Fibonacci</h2>

We’ve seen in class that calculating Fibonacc i numbers with the most straightforward recursive implementation of the function is prohibitively slow, as there is a lot of repetitive computation:

int fib(int n)

{

//base cases: F(0) = 0, F(1) = 1 if (n &lt; 2)

return n;

//definition of Fibonacci: F(n) = F(n – 1) + F(n – 2) return fib(n – 1) + fib(n – 2);

}

This recursive function sports an exponential runtime. We saw in class that we can achieve linear runtime by building from our base cases,<em>F</em>(0) = 0 and <em>F</em>(1) = 1, toward our desired result, F(n). We thus avoid our expensive and exponentially EXplOsIVe recursive function calls.

The former approach is called “top-down” processing, because we work from n down toward our base cases. The latter approach is called “bottom-up” processing, because we build from our base cases up toward our desired result, F(<em>n</em>). In general, the process by which we eliminate repetitive recursive calls by re-ordering our computation is called “dynamic programming,” and is a topic we will explore in more depth in COP 3503 (Computer Science II).

<h2>1.2          Representing huge integers in C</h2>

Our linear Fibonacci function has a big problem, though, which is perhaps less obvious than the original runtime issue: when computing the sequence, we quickly exceed the limits of C’s 32-bit integer representation. On most modern systems, the maximum int value in C is 2<sup>32</sup><em>− </em>1, or 2,147,483,647. <a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> The first Fibonacci number to exceed that limit is F(47) = 2,971,215,073.

Even C’s 64-bit unsigned long long int type is only guaranteed to represent non-negative integers up to and including 18,446,744,073,709,551,615 which is 2<sup>64 </sup><em>− </em>1. <a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> The Fibonacci number F(93) is 12,200,160,415,121,876,738, which can be stored as an unsigned long long int. However, F(94) is 19,740,274,219,868,223,167, which is too big to store in any of C’s extended integer data types.

To overcome this limitation, we will represent integers in this program using arrays, where each index holds a single digit of an integer.<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a> For reasons that will soon become obvious, we will store integers in reverse order in these arrays. So, for example, the numbers 2,147,483,648 and 10,0087 would be represented as:

Figure 1: Two numbers stored in array – LSD first

Storing these integers in reverse order makes it <em>really </em>easy to add two of them together. The ones digits for both integers are stored at index [0] in their respective arrays, the tens digits are at index [1], the hundreds digits are at index [2], and so on. <em>How convenient!</em>

So, to add these two numbers together, we add the values at index [0] (8 + 7 = 15), throw down the 5 at index [0] in some new array where we want to store the sum, carry the 1, add it to the values at index [1] in our arrays (1 + 4 + 8 = 13), and so on:

Figure 2: Calculating the sum of two numbers (LSD first)

some stuff goes here too In this program, we will use this array representation for integers. The arrays will be allocated dynamically, and we will stuff each array inside a struct that also keeps track of the array’s length:

typedef struct HugeInteger

{

// a dynamically allocated array to hold the digits // of a huge integer, stored in reverse order int *digits;

// the number of digits in the huge integer (which is // approximately equal to the length of the array) int length;

} HugeInteger;

<h2>1.3          Unsigned Integers and limits.h</h2>

There’s one final curve ball you have to deal with: there are a few places where your program will utilize unsigned integers. This is no cause to panic. An unsigned integer is just an integer that can’t be negative. (There’s no “sign” associated with the value. It’s always positive.) You declare an unsigned integer like so: unsigned int n;

Because an unsigned int is typically 32 bits (like the normal int data type), but doesn’t need to use any of those bits to signify a sign, it can eke out a higher maximum positive integer value than a normal int.

For at least one function in this assignment, you’ll need to know what the maximum value is that you can represent using an unsigned int on the system where your program is running. That value is defined in your system’s limits.h file, which you should #include from your Fibonacci.c source file, like so:

#include &lt;limits.h&gt;

limits.h defines a value called UINT_MAX, which is the maximum value an unsigned int can hold. It also defines INT_MAX (the maximum value an int can hold), UINT_MIN, INT_MIN, and many others that you might want to read up on in your spare time.

If you want to print an unsigned int, the correct conversion code is %u. For example:

unsigned int n = UINT_MAX; printf(“Max unsigned int value: %u
”, n);

Note that (UINT_MAX + 1) necessarily causes integer overflow, but since an unsigned int can’t be negative, (UINT_MAX + 1) just wraps back around to zero. Try this out for fun.<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a>

unsigned int n = UINT_MAX;

printf(“Max unsigned int value (+1): %u
”, n + 1);

Compare this, for example, to the integer overflow caused by the following:

int n = INT_MAX;

printf(“Max int value (+1): %d
”, n + 1);

<h1>2           Attachments</h1>

<h2>2.1          Header File (Fibonacci.h)</h2>

This assignment includes a header file, Fibonacci.h, which contains the definition for the HugeInteger struct, as well as functional prototypes for all the required functions in this assignment. You should #include this header file from your Fibonacci.c source file, like so:

#include “Fibonacci.h”

<h2>2.2          Test Cases</h2>

This assignment comes with multiple sample main files (fib-main01-05.c), which you can compile with your Fibonacci.c source file. For more information about compiling projects with multiple source files, see Section 4, “Compilation and Testing (CodeBlocks),” and Section 5, “Compilation and Testing (Linux/Mac Command Line).”

<h2>2.3          Sample Output Files</h2>

Also included are a number of sample output files that show the expected results of executing your program (fib-output01-05.txt).

<h2>2.4          Disclaimer</h2>

The test cases included with this assignment are by no means comprehensive. Please be sure to develop your own test cases, and spend some time thinking of “edge cases” that might break each of the required functions.

<h1>3           Function Requirements</h1>

In the source file you submit, Fibonacci.c, you must implement the following functions. You may implement any auxiliary functions you need to make these work, as well. Notice that none of your functions should print anything to the screen.

HugeInteger *hugeAdd(HugeInteger *p, HugeInteger *q);

<strong>Description: </strong>Return a pointer to a new, dynamically allocated HugeInteger struct that contains the result of adding the huge integers represented by p and q.

<strong>Special Notes: </strong>If a NULL pointer is passed to this function, simply return NULL. If any dynamic memory allocation functions fail within this function, also return NULL, but be careful to avoid memory leaks when you do so.

<strong>Hint: </strong>Before adding two huge integers, you will want to create an array to store the result. You might find it helpful to make the array slightly larger than is absolutely necessary in some cases. As long as that extra overhead is bounded by a very small constant, that’s okay. (In this case, the struct’s length field should reflect the number of meaningful digits in the array, not the actual length of the array, which will necessarily be a bit larger.)

<strong>Returns: </strong>A pointer to the newly allocated HugeInteger struct, or NULL in the special cases mentioned above.

HugeInteger *hugeDestroyer(HugeInteger *p);

<strong>Description: </strong>Destroy any and all dynamically allocated memory associated with p. Avoid segmentation faults and memory leaks.

<strong>Returns: </strong>NULL

HugeInteger *parseString(char *str);

<strong>Description: </strong>Convert a number from string format to HugeInteger format. (For example function calls, see fib-main01.c.)

<strong>Special Notes: </strong>If the empty string (“”) is passed to this function, treat it as a zero (“0”). If any dynamic memory allocation functions fail within this function, or if str is NULL, return NULL, but be careful to avoid memory leaks when you do so. You may assume the string will only contain ASCII digits ‘0’ through ‘9’, and that there will be no leading zeros in the string.

<strong>Returns: </strong>A pointer to the newly allocated HugeInteger struct, or NULL if dynamic memory allocation fails or if str is NULL.

HugeInteger *parseInt(unsigned int n);

<strong>Description: </strong>Convert the unsigned integer n to HugeInteger format.

<strong>Special Notes: </strong>If any dynamic memory allocation functions fail within this function, return NULL, but be careful to avoid memory leaks when you do so.

<strong>Returns: </strong>A pointer to the newly allocated HugeInteger struct, or NULL if dynamic memory allocation fails at any point.

unsigned int *toUnsignedInt(HugeInteger *p);

<strong>Description: </strong>Convert the integer represented by p to a dynamically allocated unsigned int, and return a pointer to that value. If p is NULL, simply return NULL. If the integer represented by p exceeds the maximum unsigned int value defined in limits.h, return NULL.

<strong>Special Notes: </strong>The sole reason this function returns a pointer instead of an unsigned int is so that we can return NULL to signify failure in cases where p cannot be represented as an unsigned int.

<strong>Returns: </strong>A pointer to the dynamically allocated unsigned integer, or NULL if the value cannot be represented as an unsigned integer (including the case where p is NULL).

HugeInteger *fib(int n);

<strong>Description: </strong>This is your Fibonacci function; this is where the magic happens. Implement an iterative solution that runs in O(n) time and returns a pointer to a HugeInteger struct that contains F(n). Be sure to prevent memory leaks before returning from this function.

<strong>Space Consideration: </strong>When computing F(n) for large n, it’s important to keep as few Fibonacci numbers in memory as necessary at any given time. For example, in building up to F(10000), you won’t want to hold Fibonacci numbers F(0) through F(9999) in memory all at once. Find a way to have only a few Fibonacci numbers in memory at any given time over the course of a single call to fib().

<strong>Special Notes: </strong>You may assume that n is a non-negative integer. If any dynamic memory allocation functions fail within this function, return NULL, but be careful to avoid memory leaks when you do so.

<strong>Returns: </strong>A pointer to a <em>HugeInteger </em>representing F(n), or NULL if dynamic memory allocation fails.

double difficultyRating(void);

<strong>Returns: </strong>A double indicating how difficult you found this assignment on a scale of 1.0 (ridiculously easy) through 5.0 (insanely difficult).

double hoursSpent(void);

<strong>Returns: </strong>An estimate (greater than zero) of the number of hours you spent on this assignment.

<h1>4           Compilation and Testing (Linux/Mac Command Line)</h1>

To compile multiple source files (.c files) at the command line:

gcc Fibonacci.c fib-main01.c

By default, this will produce an executable file called a.out that you can run by typing, e.g.:

./a.out

If you want to name the executable something else, use:

gcc Fibonacci.c fib-main01.c -o fib.exe …and then run the program using:

./fib.exe

Running your program could potentially dump a lot of output to the screen. If you want to redirect your output to a text file in Linux, it’s easy. Just run the program using the following:

./fib.exe &gt; whatever.txt

This will create a file called whatever.txt that contains the output from your program.

Linux has a helpful command called diff for comparing the contents of two files, which is really helpful here since we’ve provided sample output files. You can see whether your output matches ours exactly by typing, e.g.:

diff whatever.txt fib-output01.txt

If the contents of whatever.txt and fib-output01.txt are exactly the same, diff won’t have any output. It will just look like this:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="600d03010c10090e20051513140913">[email protected]</a>:~$ diff whatever.txt fib-output01.txt <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="ef828c8e839f8681af8a9a9c9b869c">[email protected]</a>:~$ _

If the files differ, it will spit out some information about the lines that aren’t the same. For example:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="dbb6b8bab7abb2b59bbeaea8afb2a8">[email protected]</a>:~$ diff whatever.txt fib-output01.txt

6c6

&lt; F(5) = 3

—

&gt; F(5) = 5

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="8fe2eceee3ffe6e1cfeafafcfbe6fc">[email protected]</a>:~$ _

<h2>4.1          Deliverables</h2>

Submit a single source file, named Fibonacci.c, via Webcourses. The source file should contain definitions for all the required functions (listed above), as well as any auxiliary functions you need to make them work.

Your source file must not contain a main() function. Do not submit additional source files, and do not submit a modified Fibonacci.h header file. Your program must compile and run on Eustis to receive credit. Programs that do not compile will receive an automatic zero. Specifically, your program must compile without any special flags, as in:

gcc Fibonacci.c fib-main01.c

Be sure to include your name and NID as a comment at the top of your source file.


