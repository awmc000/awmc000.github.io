---
title: Using Heron's Method to compute the square root of a natural number
layout: post
post-image: https://projectlovelace.net/static_prod/img/YBC7289.jpg
description: Implementing an ancient method of computing square roots in C.
tags:
- math
- informative
- technology
---
## From Wikipedia:
> An unknown Babylonian mathematician somehow correctly calculated the square root of 2 to three sexagesimal "digits" after the 1, but it is not known exactly how.
> The first explicit algorithm for approximating $\sqrt {S}$ is known as Heron's method, 
> after the first-century Greek mathematiician Hero of Alexandria who described the method in his AD 60 work Metrica.[2] 
> The basic idea is that if $x$ is an overestimate to the square root of a non-negative real number $S$ 
> then $\frac{S}{x}$ will be an underestimate, or vice versa, and so the average of these 
> two numbers may reasonably be expected to provide a better approximation. 


## The process
(Also from Wikipedia.)
1. Set an arbitrary starting number $x$.
2. Let $x_{n + 1}$ be the average of $x_n$ and $S$.
3. Repeat step 2 to the desired decimal accuracy.
In my first solution, any natural number's square root can be computed.
Any digits in the places after the decimal place will be printed if they exist, which is checked with the `floor()` function belonging to the C library's `math.h` header.
## My first solution, which takes a command line argument


### Source file

In a file titled `square_root.c`:


```c
/*
 * Heron's Method implementation
 *
 * Computes the square root of a given natural number
 * That is, one in the set [1, 2, 3, ..., inf].
 *
 * January 23, 2023.
 */
#define ACCURACY_ITERATIONS 100

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int main(int argc, char * argv[])
{
	if (argc < 2)
	{
		fprintf(stderr, "Did not receive a number.\n");
		exit(EXIT_FAILURE);
	}
	// get a number from the user
	int s = atoi(argv[1]);


	// set an arbitrary starting guess for x
	double x = 1;

	// repeat until accuracy desired
	for (int i = 0; i < ACCURACY_ITERATIONS; i++)
	{
		// let x_{n+1} be the average of x_n and S/x_n
		x = (x + (s / x)) / 2;
	}

	// If the number is whole, we don't want to print it with leading zeroes
	// So we use floor division to check
	double floor_x = floor(x);

	// If these values are the same, x is a whole number, eg. 16.000 == 16 evaluates true
	if (floor_x == x)
	{
		printf("%d\n",(int) x);
	}
	// If not, there is a decimal we need to print.
	else
	{
		printf("%f\n", x);
	}

	return 0;
}

```


### Shell script for compiling

In a file titled `build.sh`:


```bash
#!/bin/bash

gcc square_root.c -g -o square_root -Wall -Wextra -lm
```
## My second solution, Exercism problem submission
My workflow to solve an Exercism problem is guided by the test cases, so I have included those at the end, but note I did not write or modify them.
### Header file
In a file titled `square_root.h`:
```c
#ifndef SQUARE_ROOT_H
#define SQUARE_ROOT_H

#include <stdlib.h>
#include <math.h>

int square_root(int s);

#endif

```

### Source file
In a file titled `square_root.c`:

```c
#define ITERATIONS 100

#include "square_root.h"

int square_root(int s)
{
	// arbitrary starting guess for x
	double x = 1;

	// repeat for specified number of iterations
	for (int i = 0; i < ITERATIONS; i++)
	{
		x = (x + (s / x)) / 2;
	}

	return (int) x;

}

```

### Test cases
In a file titled `test_square_root.c`, which again, I did not modify or write:

```c
#include "test-framework/unity.h"
#include "square_root.h"

void setUp(void)
{
}

void tearDown(void)
{
}

static void test_root_of_1(void)
{
   TEST_ASSERT_EQUAL_UINT16(1, square_root(1));
}

static void test_root_of_4(void)
{
   
   TEST_ASSERT_EQUAL_UINT16(2, square_root(4));
}

static void test_root_of_25(void)
{
   
   TEST_ASSERT_EQUAL_UINT16(5, square_root(25));
}

static void test_root_of_81(void)
{
   
   TEST_ASSERT_EQUAL_UINT16(9, square_root(81));
}

static void test_root_of_196(void)
{
   
   TEST_ASSERT_EQUAL_UINT16(14, square_root(196));
}

static void test_root_of_65025(void)
{
   
   TEST_ASSERT_EQUAL_UINT16(255, square_root(65025));
}

int main(void)
{
   UnityBegin("test_square_root.c");

   RUN_TEST(test_root_of_1);
   RUN_TEST(test_root_of_4);
   RUN_TEST(test_root_of_25);
   RUN_TEST(test_root_of_81);
   RUN_TEST(test_root_of_196);
   RUN_TEST(test_root_of_65025);

   return UnityEnd();
}

```

## Conclusion
This was an interesting although simple algorithm to learn and I look forward to adding more ancient algorithms to my collection.
**Thank you for reading!** 

If this interested you, ask about my Sieve of Eratosthenes solution I described on Youtube. Sooner or later I will write a blog post about that one too.
