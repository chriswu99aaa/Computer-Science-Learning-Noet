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
```
public boolean isUniqueChars(String str)
{
    if(str.length()>256) return false; //This precondition check applies in so many cases

    /*
    *The range of character set for ASCII value is 256. If the value is given
    * as UNICODE then set the array size to be 2^16
    */
    boolean[] char_set = new boolean[256]; 
    for(int i=0; i<str.length(); i++)
    {
        int int_val = str.charAt(i);
        if(char_set[int_val]){
            return false;
        }
        char_set[int_val] = true;
    }
    return true;
}
```
The thought process is first check if the given string is longer than 256. Secondly, instantiate an array of boolean of size 256.
Thirdly, we loop through the given string. we get the ascii value of a char at position i, and check if the boolean array at position
int_val, which is the ascii of the char at positon i in the string, is true. If it is, it implies that there was a previous assignment to
this position, so return false; otherwise, we assign true to the boolean array at position int_val. If after looping thorough the whole array
there were no fasle, then return true;

For check if a string is all unique character we can set a boolean array to be size 256 assigning each character a 
spot in the array.
The time compexity is O(n) and spaceis O(1).

1.2 Implement a function void reverse(char* str) in C or C++ which reverses a null-terminated string.
```
//This implementation is in C
void reverse(char *str)
{
    char* end= str;
    char tmp;
    if(str){
        while(*end){ /*fidn end of the string */
            end++;
        }
        end--; /* set one char back, since last char is null */

        while(str < end){
            tmp = *str;
            *str++ = *end;
            }
      }
 }
```
1.3 given two strings ,write  method to decide if one is a permutation of the other.

```
public String sort(String s)
{
    char[] char_arr =   s.toCharArray();
    java.util.Arrays.sort(char_arr);
    String str  = new String(char_arr);
    return str;
}
public boolean isPermutation(String str1, Sring str2)
{
    if(str1.length() != str2.length() return false;
    
    return sort(str1).equals(sort(str2));
}
```
We aim to check if two given strings are permutations of one another. The thought is that a permutation must be a 
bijection so they must have the same length, so we check if they have same length. Secondly, we introduced another
function sort. This used the sorting function in Arrays. When sorting can we use the sort function provided by JDK or
we need to implements our own?

After sorting the string, we can use String.equals() to compare if two strings are the same. This implementation is very simple and clean if 
we need to consider efficiency we use the alternative below.

```
public boolean isPermutation(String s, String t)
{
    if(s.length() != t.length()) return false;
    
    int[] letter = new int[256] //here we use the idea of assiging a array of size 256 for ascii
    char[] char_set = s.toCharArray();
    
    for(char c: char_set)
    {
        letter[c]++; // type char c will be converted to integer in the context. This is type cascade
    }
    for(char c : t)
    {
        int frequency = letter[c]--;
        if(frequency < 0) return false; // This works fine because all primitive type except boolean will be zero by default.
    }
}
```
This alternative approach use the idea of counting each character because there must be an bijection between two strings
so the amount in the former must be euqal to that of in the latter. So we create an array of size 256, and convert each
characetr into ascii value and increment the frequency in the integer array at the position corresponding to  the ascii value.
If they were a permutation, then all word frequencies must be matched. If there is a negative vlaue, then itimplies that there exist a character in the second string which doesn't exist in the first array.

**Conclusion**
We have done two ways for finding if two given strings are permutations of one another by comparing the sorted strings and counting word frequencies of two 
strings. The former is simpel and clean and the latter is efficient.

**Note** The idea od 256 size array can also be used to check if a string is a substring of the other by defining an integer array of size 256
and couting the frequency of the longer string. Then do the character decrement approach as shown above. If there is a negative frequecy then
the shorter string is not a substring of the other sicne there exist some character in the shorter which don't exist in the longer string.


1.4 Write a method to replace all spaces in a string with '%20'. 
Example:
Input: "Mr Joghn Smith     "
Output: "Mr%20John%20Smith"

```
public void replaceSpace(char[] str, integer length)
{
    int space = 0;
    int new_length;
    for(char c : str)
    {
        if(c == ' ')
        {
            space++;
        }
    }
    new_length = length + 2*space;
    str[new_length] = '\0';  //this symbol makes the string terminate a position of new_length. This is the buffer
    for(int i=length-1; i>=0; i--)
    {
        if(str[i]==' ')
        {
            str[new_length-1] = '0';
            str[new_length-2] = '2';
            str[new_length-3] = '%';
            new_length = new_length -3;
        }else{
            str[new_length-1] = str[i];
            new_length = new_length-1;
        }
    }
}
```
**Note** A common approach in string manipulation problems is to edit the string starting from the end and work backwords. This is useful because we
have extra buffer at the end, which allows us to change character without worring about what we're overwriting.

This approache use two scan, first counting the amount of white space to determine the final length of new array and second to edit the char array.

This repalce function example can be extended to editing strings i.e. removing characters from the string, adding characterrs to the string or 
replacing characters with other characters. We can use this two scan approach.

1.5 Implement a methdo to perform basic sring compression using the counts fo repeated characters. For example abccccaa would become a2b1c5a. If the "compressed" string would not become smaller than the original string, your method should return the original string.

**Easy and bad solution**
```
public string compress(String str)
{
    String mystr = "";
    char last = str.charAt(0);
    int count = 1;
    for(int i=1; i<str.length(); i++)
    {
        if(str.charAt(i) == last)
            count++;
        else{
            mystr += last + "" + count;
            last = str.charAt(i);
            count = 1;
        }
    }
    return mystr + last + count;
}
```
**Note** This is slow because string concatenation operates in O(n^2) time.

Alternatively, we can use array to solve this.

```
public int countCompress(String str)
{
    int size = 0;
    char last = str.charAt(0);
    count =1;
    for(int i =1; i<str.length(); i++)
    {
        if(str.charAt(i) == last)
            count++;
        else{
            size += 1 + String.valueOf(count).length(); //The char occupies one slot, and the rest is the length of count.
            count = 1;
            last = str.charAt(i);
        }
    }
    size = 1 + String.valueOf(count).length();
    return size ;
}

public int setChar(char[] array, char c, int index, int count)
{
    array[index] = c;
    index++;
    char[] c_arr = String.valueOf(count).toCharArray();
    for(char c : c_arr)
    {
        array[index] = c;
        index;
    }
    return index;
}

public String compress(String str)
{
    int size = countCompress(str);
    if(size > str.length())
        return str;
    
    char[] array = char[size];
    char last = str.charAt(0);
    int count = 1;
    
    for(int i=1; i<str.length(); i++)
    {
        if(str.charAt(i) == last)
              count++;
        else{
            index = setChar(array, last, index, count);
            last = str.charAt(i);
            count = 1;
        }
    }
    index = setChar(array, last, index, count);
    
    return  String.valueOf(array);
}
```

1.6 Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image
by 90 dgrees. Can you do this in place?
```
public void transpose(int[][] A)
{
    for(int i=0; i<A.length; i++)
    {
        for(int j=i+1; j<A.length; j++)
        {
            int tmp = A[i][j];
            A[i][j] = A[j][i];
            A[j][i] = tmp;
        }
    }
}

public void swap(int[] A int a, int b)
{
    int tmp = A[a];
    A[a] = A[b];
    A[b] = tmp;
}

public void rotateMatrix(int[][] A)
{
    A = transpose(A);
    for(int[] arr:A)
    {
        for(int i=0; i<arr.length/2;i++)
        {
            swap(arr,i,arr.length-1-i);
        }
    }
}
```




















































 



















