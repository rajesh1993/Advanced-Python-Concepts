---
layout: page
---

## Generators

Whenever you want to return an iterable like a list of numbers or strings from a function, Generators are the right way to go!

For example, let's say I wanted to fetch the first _n_ odd numbers. Then, for each number in the list, I want to find out how many die rolls I need so that the sum of the rolls reaches exactly or over the current number.

Typically the program to fetch the first _n_ odd numbers would look like this:

```python
# Fetches the first n odd numbers as a list.
def first_n_odd(n):
    result = []
    for num in range(1, 2 * n, 2):
        result.append(num)
    return result
```

Now to use it to find out the number of die rolls required would be:

```python
import random

# Counts the number of die rolls required to reach given target
def count_die_rolls_to_target(target):
    rolls_sum = 0
    rolls_count = 0
    while rolls_sum < target:
        rolls_sum += random.randint(1, 6)
        rolls_count += 1
    return rolls_count
```

Now, finally I use the above two functions in tandem to find out the number of rolls required for each of the odd numbers I generated.

```python
# Prints number of rolls required for first n odd numbers
def rolls_distribution(n):
    # Compute rolls for each odd number from 1 to n
    for odd_num in first_n_odd(n):
        print("Rolls required to reach %d is %d" % (odd_num, count_die_rools_to_target(odd_num))
```

Now, that was simple enough. But what if we had _n_ to be a very large number, let's say in the billions. So that is around 10^9 numbers that we have to do this exercise for and return a list. Imagine the amount of memory required to store so many numbers.

What if there was a way to get each odd number on the fly and then compute the number of rolls instead of storing all 10^9 numbers in a list and then processing it. Here comes the magic of generators.

Let us see how we can rewrite our odd number generation function.

```python
# Generates the first n odd numbers.
def first_n_odd(n):
    for num in range(1, 2 * n, 2):
        yield num
```

Wait a minute. Where did _return_ go and what is this _yield_ doing there? Also, note that there is no result list now.

All we have done is exactly what we set out to do. By using _yield_, we return only one value at a time each time the function is called. So now we no longer have a list storing all one billion numbers. Phew, memory saved. Ok, this is good news. But what happens at the function call? Let's see that function again:

```python
# Prints number of rolls required for first n odd numbers
def rolls_distribution(n):
    # Compute rolls for each odd number from 1 to n
    for odd_num in first_n_odd(n):
        print("Rolls required to reach %d is %d" % (odd_num, count_die_rools_to_target(odd_num))
```

This function call still works! How does it work though?

The _for_ loop calls the _next()_ function on the iterable in the statement. In our case it is the generator object that the __first_n_odd__ function returns. So each time the next value _generated_ is _yielded_ by the function.

The whole program could be written even more simply using list comprehension as follows:
```python
# Prints number of rolls required for first n odd numbers
def rolls_distribution(n):
    # Generate first n odd numbers as a generator object
    first_n_odd = (x for x in range(1, 2 * n, 2))
    # Compute rolls for each odd number from 1 to n
    for odd_num in first_n_odd:
        print("Rolls required to reach %d is %d" % (odd_num, count_die_rools_to_target(odd_num))
```
Instead of using the usual [] we use the tuple brackets () for generators while doing list comprehension.

That was easy right! Where do we use this though? Think of a scenario where we would require a huge number of items in a list. Some possible scenarios are as follows:

- If you are doing a Natural Language Processing (NLP) task where you are analysing books' data to make the computer understand the story and write a story of its own! You will want to process each word of each book in your dataset. So, instead of storing all those books in memory you can use a generator function to give you one word at a time. Or if you want lesser granularity, one book at a time.

Come up with your own interesting scenario to use generators and see if they work well with for that application.

Thanks to [__Sebastiaan Mathôt__](https://www.youtube.com/channel/UC6HfeAa0vWeSWS6IcNAjZ2A) for teaching me this concept.

His detailed video is here:

<iframe width="315" height="175" src="https://www.youtube.com/embed/Ut0-_eMVakU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> <iframe width="315" height="175" src="https://www.youtube.com/embed/x3N3JmgjXxg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



[back](./)
