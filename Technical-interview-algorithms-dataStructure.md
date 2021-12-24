# Data Structures

## Arrays and Strings
**Arays questions and tring qeustions are often interchangesable. This means one question asked in array might be asked as a string question**

### Hash Tables
**Definition: A hash table is a data strcutre that maps keys to values for highly effcient lookup. Hash tbale has an underlying array and a hash function**
The common strategies handling collisions are open addressing and seperate chaining. The former use linear probing, quadratic probing, and double hashing. Double hashing
needs two hash functions to rehash the index. The latter used a linked list to store the the collided element.

Alternatively, we can use bineary tree to implement the hash table, so we can guarantee an O(log n) time complexity and we use less space, since a large array no longer needs
to be allocated in the very beginning.

I need to look up how to implement hash table in a binary tree. They ar eone of hte most common data strcutres for interviews.

### ArrayList (Dnamically Resizing Array)
**Definition: An ArrayList is an array that resizes itself as needed while still providing O(1) access. When the array is full we double the 
size of the array.**

Each doubling takes O(n) (happened when the table is full), but an amortised analysis can show that each operation only takes O(1) time complexity.
We need to record the load factor of the ArrayList so that we can have a nice balanced between amount of elements and speed for resizing. I like to make it 75%

**Note**
1. The difference between ASCII and UNICODE:
* Unicode is the universal character encoding used to process, store, and facilitates the interchange of text data in any language.
* ASCII is used for representations of text such as symbols, letters, digits, etc in computers

ASCII: American Standard Code for Information Interchange. Each letter is assgiend to an specific integer ranging from  0 to 127. 
One bit represents a switch state: on and off. Characers set represents is 8 bits (one byte) with 256 difference characters doubling the 
size of ASCII set. (2^8=256)

UNICODE: provides an unique way to define every characer in every spoken language in the world by assigning it an unique integer. Unicode uses two bytes to represent
all characters in the world

**Difference between UNICODE and ASCII**
1. UNICODE  supports wider range of characters than ASCII, since the latter only support 127 distints characters
2. ASCII uses oen byte whereas UNICODE used two bytes.

1.1 Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data strucutres?






















