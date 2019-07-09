# Hash-Tables-Notes

[Lecture 1: How Arrays Work](#Lecture-I)  
a. [Arrays](#Arrays)  
b. [Arrays.py](#Arrays.py)  
c. [Resizing Arrays](#Resizing-Arrays)  
d. [Read Arrays](#Read-Arrays)  
e. [Insert Into Arrays](#Insert-Into-Arrays)  
f. [Append to Arrays](#Append-to-Arrays)  
g. [Remove from an Array](#Remove-from-an-Array)  
h. [Pop Arrays](#Pop-Arrays)  
i. [Doubly Linked Lists](#Doubly-Linked-Lists)  
j. [Hash Tables](#Hash-Tables)  
k. [Additional Resources](#Lecture-I-Additional-Resources)  

<br>
<br>
<br>

[Lecture 2: Hash Tables](#Lecture-II)  


<br>
<br>

If you found these notes helpful and want to show appreciation to the author, [coffee donations](buymeacoff.ee/G1stPBuYU) are much loved.  


# Lecture I

[Lecture I Recording](https://www.youtube.com/watch?v=MOonWip19TM&feature=youtu.be)

## Arrays

What is an array and how do they work?  

- They have a fixed length/size  
- A sequence of contiguous objects of the same type, storing data  
- Index based with a value  
- Time and Space efficient  
- Fast at inserting to the end, slow at inserting to the beginning (because all objects in the array are shifted one space to the right if there is available space OR a new copy of the array is made if there is no available space)  

> 01 01 was a race horse  
> 10 10 was one too  
> 01 01 won one race  
> 10 10 01 01 10  

An array allocates a certain amount of memory when they are created.  

Array lookup begins at `[0]` because it's zero pointer sizes away from the pointer at the beginning of the sequence. While it's harder for people, it's easier for computers to do the math.   

> Index * size of type = array[index]  

So, 0 * 4 bits = 0. If we started with 1 instead of 0, that would require an additional operation to find the starting point beause 1 * 4 = 4, not 0.  

array[5] would point to the 20th bit because 5 * 4 = 20.  

If we know that we want an array of 3, 12 bits of memory will be set aside for this array (3 * 4 = 12).  

If we want to decrease the size of that array, we can easily indicate that the memory for that array is _less_ than 12 bits.  

But if we want to _add_ to the array, we have to first check _if_ the memory next to the end of the current array is already allocated.  

If that memory is not already allocated, we can use it to expand the array. But if it is taken, how do we expand the array?  

Because arrays must be contiguous (unlike linked lists), we can't just allocate the next closest available bits of memory.

Depending on if the language manages memory for us (like Python), we may have to do this manually.  

We first would need to find a larger memory block that fits the new array length, then copy the original array data into that new memory block. Afterwards, we release the original block of memory by marking it as free (or able to be collected by the garbage collector).  

Typically that data is not actually cleared. It's just marked as free to re-use. When we get rid of old hard drives or other memory storage devices, it's best to fully destroy anything that contained sensitive data because it may still be written to memory if it was not memory that was cleared to re-use. De-allocation is not the same as erasing, despite being "deleted" according to the computer.  

Not all languages de-allocate automatically. In Python it does, but in C, you have to do it manually (or risk a memory leak).  


## Arrays.py

Let's dive into writing code. We'll use the `arrays.py` file in the [Hash Tables Repo](https://github.com/LambdaSchool/Hash-Tables) -- copied into this repo for simplification.  

> Do not use any of the built in array functions for this exercise

We want to avoid using the magically built in functions that make for cool one liner solutions, in order to fully understand what's happening under the hood.

We'll start with our constructor:

```
class array:
    def __init__(self):
        # Your code here
        pass
```

We're going to also create functions that allow us to resize, remove, pop, add to the array, and more.  

_(Due to losing the C curriculum, we will not discuss memory management because Python does it automatically -- but if you learn C, you need to learn how to handle memory.)_  

What do we need to add to our array constructor to make it work?  

We need to know how much memory to allocate for the array (capacity). Capacity will be passed in and is the "maximum" size that the array _could_ become.  

We also need the actual current size, which begins at 0 when an array is initialied.  

Lastly, we need the elements (buckets of actual memory) that will start out with nothing (`none`) in them. 

```
class array:
    def __init__(self, capacity):
        # We need to set the capacity
        self.capacity = capacity # Maximum size the array can become

        # We also need the actual current size
        self.count = 0 # Current size being used

        # We need to create the empty cells within the block of memory
        self.elements = [None] * capacity
```

## Resizing Arrays

If we want to increase the size of the array, it's best practice to simply _double_ it. But why? Why not just add one bit of memory?  

This makes the frequency of needing to change the size less often.  

Isn't that wasteful of memory? Doubling increases in size very quickly. But memory is cheap, while computational processing is expensive. If we give up memory to reduce processing expense, that's a valuable trade off.   

Typically we re-allocate memory, not when the memory is full, but usually at a cap around 70-80% full preventatively.  

To double the array size...

```
def resize_array(array):
    # Double the old capacity
    new_capacity = array.capacity * 2

    # Re-allocate the memory
    new_elements = [None] * new_capacity

    # Copy the elements over
    # We use count instead of capacity because we only need to do it for the memory actually being used
    for i in range(array.count):
        new_elements[i] = array.elements[i]

    # Copy over our changes
    array.elements = new_elements
    array.capacity = new_capacity
```


## Read Arrays

Now we need to write a function for reading an array, by finding the value of a certain index.  

The first thing to check though is to make sure that the index exists within the array by testing the boundaries.  

We need to make sure that the index is not outside of `0` to `array.count`.  

```
# Return an element of a given array at a given index
def array_read(array, index):
    # Throw an error if array is out of the current count
    # Why? To recognize an out of bounds exception

    # How do we know if we're out of bounds?
    if index > array.count:
        print("Error: out of bounds")
        return None
    
    # Otherwise, return the index value
    return array.elements[index]
```


## Insert Into Arrays

Next, we want to write a function that inserts data into an array.  

We'll start by using the same error handling as before to ensure that we're in range, but removing the `>=` in favor of just `>` to avoid looking outside of the array.

```
def array_insert():
    # Throw an error if array is out of the current count
    if index >= array.count:
        print("Error: out of bounds")
        return None

    # Resize the array if the number of elements is over capacity

    # Move the elements to create a space at 'index'
    # Think about where to start!

    # Add the new element to the array and update the count
    pass
```

How will we resize the array? First we check to see if the number of elements is greater than the capacity.  

We could check if `array.count >= array.capacity` OR if `array.count >= array.capacity * 0.80` -- to proactively prevent it from nearing capacity.

But today we'll check if `array.capacity <= array.count` and double the array if that condition is hit:  

```
def array_insert(array, index):
    # Throw an error if array is out of the current count
    if index > array.count:
        print("Error: out of bounds")
        return None

    # Resize the array if the number of elements is over capacity
    if array.capacity <= array.count:
        # Create a new array that is doubled in size with our previously written resize_array function
        resize_array(array)

    # Move the elements to create a space at 'index'
    # Think about where to start!

    # Add the new element to the array and update the count
    pass
```

Next we need to move the elements one space to the right, to create a space at index. We can use a `for in range` loop that allows us to use `start, stop[ step]` so we can start at the _end_ of the loop to create a space for each previous element.  

> 22, 34, 76, 43, 82, 91, xx, xx, xx  

We can see our array above has 3 spare spaces at the end of the allocated memory. Our loop would look like:

> 22, 34, 76, 43, 82, xx, 91, xx, xx  
> 22, 34, 76, 43, xx, 82, 91, xx, xx  
> 22, 34, 76, xx, 43, 82, 91, xx, xx  

And so on until we get to the index where the new element is being inserted.  

```
    # Move the elements to create a space at 'index'
    # Everything to the right of index needs to move a space to the right
    for i in range(array.count, index, -1):
        array.elements[i] = array.elements[i-1]
```

This is a rare time where we can use `array.count` not `array.count - 1` because we _do_ want to move everything over one to the right.

Lastly, we'll add in the new element to the indicated index and adjust the `array.count`:

```
def array_insert(array, element, index):
    # Throw an error if array is out of the current count
    if index > array.count:
        print("Error: out of bounds in array_insert")
        return None

    # Resize the array if the number of elements is over capacity
    if array.capacity <= array.count:
        # Create a new array that is doubled in size with our previously written resize_array function
        resize_array(array)

    # Move the elements to create a space at 'index'
    # Everything to the right of index needs to move a space to the right
    for i in range(array.count, index, -1):
        array.elements[i] = array.elements[i-1]

    # Add the new element to the array and update the count
    array.elements[index] = element
    array.count += 1
```

## Append to Arrays

Rather than build this function again, we can use the insert method that we've already built.

Looking at this example again, where is index of `array.count`?

> 22, 34, 76, 43, 82, 91, xx, xx, xx 

It would be the blank spot after 91 because array index starts at 0. So array[6] is `xx` not `91`. If we want to add to the end of the array, we'll use `array.count`:


```
# Add an element to the end of the given array
def array_append(array, element):

    # Hint, this can be done with one line of code
    # (Without using a built in function)

    array_insert(array, element, array.count)
```


## Remove from an Array

Now we want to remove an element from the array but only IF it exists. We need to throw an error if it does not.  

We can use a for loop to check each value in the array. But what do we do if we find the element?

```
def array_remove(array, element):
    for i in range(array.count):
        if array[i] == element:
            
```

We can't just replace it with `None` because arrays must be contiguous. Instead, we might do the same process as `insert` but backwards, shifting everything over to the left.

We can store a value that shows whether or not removed is true, to change to true if the element is found so that the loop goes through to move elements forward:

```
def array_remove(array, element):
    removed = False
    for i in range(array.count):
        if removed: 
            # if removed is True, we should...?
        elif array.elements[i] == element:
            removed = True
```

If the element is found and removed becomes True, we then want the loop to set element at `i-1` to `i`, to shift the elements to the left:

```
if removed: 
            array.elements[i-1] = array.elements[i]
```


At the end, we'll check if removed is True or False, to render an error if the element was not found. We also need to decrement the count of the array and set the last index to None.

If we look at the example array again and try to remove 43:

> 22, 34, 76, 43, 82, 91, xx, xx, xx   
> 22, 34, 76, 82, 82, 91, xx, xx, xx   
> 22, 34, 76, 82, 91, 91, xx, xx, xx   

Now we'll lower the count by 1, so `array.count` = 5:

> 22, 34, 76, 82, 91, 91, xx, xx, xx    

And then set `array.count` (which is array[5], or the second `91`) to `None`:

> 22, 34, 76, 82, 91, xx, xx, xx  

With code, that will look like:

```
def array_remove(array, element):
    removed = False
    for i in range(array.count):
        if removed: 
            # if removed is True, we should...?
            array.elements[i-1] = array.elements[i]
        elif array.elements[i] == element:
            removed = True
    
    if removed:
        array.count -= 1
        array.elements[array.count] = None
    
    else: 
            print(f"Error: {str(element)} not found.")
```


## Pop Arrays

We need to remove an element at a given index and then return it; then shift every element after that index to fill in the gap.

First we'll error handle in case the index is not in range. This time, we'll set the error to be `>=` instead of `>` because we can't pop the element after the last index: 

```
def array_pop(array, index):
    # Throw an error if array is out of the current count
    if index >= array.count:
        print("Error: out of points in array_pop")
        return None
```

Now we need to set the return value and shift everything over:  

```
    # Set the return value of the number being popped so it's stored before being removed
    return_value = array.elements[index]

    # Make a for loop to shift elements over
    # Start one after the index and end at array.count (because it stops at that given position without including it)
    for i in range(index + 1, array.count):
        array.elements[i - 1] = array.elements[i]
```

Remember, that `for i in range()` is _inclusive_ of the first given element (the start) and _exclusive_ of the last given element (the end). 

Lastly, we need to update the `count` and set the last element to `None`, then return the requested `return_value`:

```
def array_pop(array, index):
    # Throw an error if array is out of the current count
    if index >= array.count:
        print("Error: out of points in array_pop")
        return None

    # Set the return value of the number being popped so it's stored before being removed
    return_value = array.elements[index]

    # Make a for loop to shift elements over
    # Start one after the index and end at array.count (because it stops at that given position without including it)
    for i in range(index + 1, array.count):
        array.elements[i - 1] = array.elements[i]

    array.count -= 1
    array.elements[array.count] = None

    return return_value
```


## Doubly Linked Lists

We'll use the `doubly_linked_list.py` file in the [Hash Tables Repo](https://github.com/LambdaSchool/Hash-Tables) -- copied into this repo for simplification.    

When would we use a doubly linked list v a dynamic array?  

We use arrays when the speed of an index lookup is needed, but linked lists when you need to resize the data and don't care about caching.  

The file (also from the Hash Tables Repo but copied here) `compare.py` has a series of tests to compare the time efficiency of different operations with linked lists and arrays.  

On your own, you can experiment with that file to make conclusions about Big O as it relates to both of those data structures in different operations.  


## Hash Tables

What are good qualities of a hash function?  

- An input must have a consistent output (always the same) and it's one-way.  
- Creating the hash should be simple but recreating it going backwards should be effectively impossible. Salting, for example, adds complexity to prevent backwards recreation.  
- It should be evenly random to avoid collisions

A hash function takes a value and runs a hashing operation onto it.

We can use a string as an index in an array (as a label to assign value to something). Like in this diagram:  

![Hash Table Example](./HashTableImage.png "Hash Table Example")

The string `John Smith` is used as the index `2` in this array, to point to the element bucket `521-1234`.  

A hash table converts the string into an index. We create an oversized array (because memory and storage are cheap, to save computing power) and set the index as a label to the corresponding integer.  

This results in an O(1) run time for finding things in the hash table.  

Imagine if we had a bunch of budget categories. In a spreadsheet, we diagram it with a series of index numbers corresponding to the budget's numerical values. Remembering which column (index) corresponds with which part of the budget would be nearly impossible if there are many categories.  

Using a hash table, we can keep a string reference to each index to make that simpler, like:  


| Category      | Index | Budget $$  |
| ------------- |:-----:| ----------:|
| Groceries     | 0     |       $500 |
| Gas Money     | 1     |       $150 |
| New Clothes   | 2     |       $100 |
| Pet Supplies  | 3     |        $25 |
| Toiletries    | 4     |        $50 |


For [today's assignment](https://github.com/LambdaSchool/Hash-Tables/tree/master/basic_hashtable), avoid re-sizing and handling collisions, which we'll cover in the next lecture.  


# Lecture I Additional Resources

[Hash Tables: Intro Video](https://youtu.be/z07XGvC9D4c)  

[Hash Tables: Collisions and Resizing Video](https://youtu.be/u38jSupgQvU)  


[What are Hashtables and Hashing Algorithms?](http://www.goodmath.org/blog/2013/10/20/basic-data-structures-hash-tables/)  

[3 Common Hash Functions](http://www.cse.yorku.ca/~oz/hash.html)  

[Breaking Down an Efficient Hash Function](http://www.azillionmonkeys.com/qed/hash.html)  



<br>
<br>
<br>
<br>

# Lecture II

[Lecture II Recording]()  

## 