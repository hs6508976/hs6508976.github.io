---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/AI_primes.png"
tags: [Python, Primes]
---

In this post I'm going to discuss my first project. In this, I defined a function that can find all of the prime numbers below an input number. It can print a list of the primes and also provide the number of primes.

For a little bit of context, a prime number is one that is only divisible by 1 and itself. 

Let's get into it!

---

First, we want to test the function with a relatively low number to see if the function will work. Let's go for 20.

```ruby
n = 20
```

The smallest prime number is 2, we want to start by creating a list to 'check' every number up to the limit that we've set. The range function needs to be set to n+1 as range is non-inclusive of the upper limit.

To complete this project, we're going to use a set, which will allow us to remove elements as the code cycles through the numbers.

```ruby
number_range = set(range(2, n+1))
```

We're also going to create a list to store all of the prime numbers that are found during the check

```ruby
primes_list = []
```

Eventually, we'll use a while loop to run through this all automatically, but I just like to check that the code is working manually first.

So, we have a set of numbers (number_range) to check all integers between 2 and 20. We will take the lowest number of this set (2), which we know is a prime, and add it to the list of primes. To do this, the *pop* method is used.

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```
Using pop allows us to assign this to the object called **prime** it will *pop* the first element from the set out of **number_range**, and into **prime**

```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

As mentioned before, the lowest number in the list has to be prime as it isn't divisible by anything. This is obvious with 2, but the power of it will be shown soon.

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

Now, we'll generate a list of the multiples of 2 up to 20. I'll save these into a set (the reason for which will be revealed), which I'll simply call **multiples**

```ruby
multiples = set(range(prime*2, n+1, prime))
```

The prime has been removed, but any multiples of it are, clearly, not prime numbers. Here, a range is used to take the first multiple of the prime number, and the step is used to generate a set of all multiples up to our upper limit. For instance, here it takes 4, 6, 8...

Lets have a look at our list of multiples...

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

Now, it's time to use the special set functionality **difference_update** which removes any values from the number range that are multiples of the number being checked. This is because, as discussed, they are clearly not prime numbers.

Before applying the **difference_update**, let's look at the two sets.

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

**difference_update** works in a way that will update one set to only include the values that are *different* from those in a second set

To use this, we put our initial set and then apply the difference update with our multiples

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```

When we look at our number range now, all values that were also present in the multiples set have been removed as we *know* they were not primes

This is amazing!  We've made a massive reduction to the pool of numbers that need to be tested so this is really efficient. It also means the smallest number in our range *is a prime number* as we know nothing smaller than it divides into it...and this means we can run all that logic again from the top! We can now put the while loop in place

Here is the code, with a while loop doing all the hard work.

Let's run it for any primes below 1000...

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n+1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime*2, n+1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we've got!

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

We can now get a quick summary of our findings. Let's get the number of primes found and the largest prime in the list!

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

Amazing!

Finally, we're going to put all of the code into a function to allow us to simply call the function for different numbers.

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n+1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime*2, n+1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can jut pass the function the upper bound of our search and it will do the rest!

Let's try this with a large number...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

I found this pretty cool, hope you did too!

---

###### Important Note: Using pop() on a Set in Python

In the real world - we would need to make a consideration around the pop() method when used on a Set as in some cases it can be a bit inconsistent.

The pop() method will usually extract the lowest element of a Set. Sets however are, by definition, unordered. The items are stored internally with some order, but this internal order is determined by the hash code of the key (which is what allows retrieval to be so fast). 

This hashing method means that we can't 100% rely on it successfully getting the lowest value. In very rare cases, the hash provides a value that is not the lowest.

The simplest solution to force the minimum value to be used is to replace the line...

```ruby
prime = number_range.pop()
```

...with the lines...

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

...this forces the identification of the lowest number in our range, before popping that out.

However, because we have to sort the list for each iteration of the loop in order to get the minimum value, it's slightly slower than what we saw with pop()!


