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
If they were a permutation, then all word frequencies must be matched. If there is a negative vlaue, then it implies that there exist a character in the second string which doesn't exist in the first array.

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
**Note** 
Rotate anti-clockwise will make this swap  first, and then transpose. When manipulating an matrix like multi-dimensional array, we can introduce some intermediate
steps to approach the final state. Also some Linear algebra knowlege regarding linear transformation can be used.


1.7 Write an algorithm such that if an element in an MxN matrix is 9, its entire row and column are set to 0.

1. Scan all places where exist a zero, and store that row and column position into boolean arrays. It's common to store into an MxN array, but at this 
case we don't care the exat location of zero, but only the row and column information of the zero, so we create two boolean array of size A.length and A[0].length
2. Set the row and column where boolean arrays has true to be zero.


```
public void setZero(int[][] A)
{
    boolean[] row = new boolean[A.length];
    boolean[] column = new boolean[A[0].length];
    
    for(int i=0; i<A.length; i++)
    {
        for(int j=0;j<A[0].length; j++)
        {
            if(A[i][j] == 0)
            {
                row[i] = 0;
                column[j] = 0;
            }
        }
    }
    for(int i=0; i<A.length; i++)
    {
        for(int j=0; j<A[0].length; j++)
        {
            if(row[i] || column[j])
            {
                A[i][j] = 0;
            }
        }
    }
}
```
This time complexity is O(n^2) and space complexity is O(N). 

If we were to set some value in multi-dimensional array, we can make another arrays and flag the position where  changes were needed.
A good example of concerning space is this. We reduce the two dimentional array to be two one dimensinoal arrays reducing space complexity
from O(MN) to be O(N). This can be done because we don't care the exact location of zero but rather the rows and columns of zeros.


1.8 Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if 
s2 is a rtation of s1 usign only one cal to isSubstring(e.g. "waterbottle" is a rotation of "erbottlewat"

```
public boolean isRotation(Strin s1, String s2)
{
    int len = s1.length();
    
    if(len == s2.length() && len>0)
    {
        String s1s1 = s1 + s1;
        return isSubstring(s1s1, s2);
    }
    return false;
}
```
Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters.

Method 1: Brute force approache.
```
public int lengthOfLongestSubstring(String s) {

		int n = s.length();
		int result = 0;
		for(int i=0; i<n; i++)
		{
				for(int j=i; j<n; j++)
				{
						if(check(s, i, j))
						{
								result = Math.max(result, j-i+1);
						}
				}
		}
		return result;
}

private boolean check(String s, int start, int end)
{
		int[] chars = new int[128];

		for(int i=start; i<=end; i++)
		{
				char c = s.charAt(i);
				chars[c]++;
				if(chars[c] >1) return false;
		}
		return true;
}
```
The Brute force approach used two pointers one record the start of a substring, and    another record the extending index of a substring. The check method use the direct access table to count the frequency of of characters in the substring from i to j.

This problem is similar to the first problem I have done on CtCi, which asked me to find if allcharacters in a string are unique. This question ask the longest length of a substring which has no repetition. We can rephrase this qeusiton to be the longest substirng in which all characters are unique. This reminds us the direct access table and the method used for checking duplicate.

Method 2: Sliding window approache
```

    public int lengthOfLongestSubstring(String s) {
            //sliding window approach
        int[] chars = new int[128];
        
        int left = 0;
        int right = 0;
        int result = 0;
        
        while(right<s.length()){
            char r = s.charAt(right);
            chars[r]++;
            while(chars[r]>1){//contract the window
                char l = s.charAt(left);
                chars[l]--;
                left++;
            }
            result = Math.max(result, right-left+1);
            
            right++;
            
        }
        return result;
    }
```
We define a derct access table of size 128, and keep two pointers. left and right within  which we form substrings. We extend the right index until. it reaches the end of the stirng, and we count the frequency of the character occurence in the substring, we one of them is larger than 1, then there is a duplicate, we need to contract the window by the left pointer to the right and decrement the occurence. Finally, we calculate the maximum of the curent window length by right-left+1 and the previous best.

Method 3: Sliding window optimised
```
   public int lengthOfLongestSubstring(String s) {
            //sliding window approach
        Integer[] chars = new Integer[128];
        
        int left = 0;
        int right = 0;
        int result = 0;
        
        while(right<s.length()){
            char r = s.charAt(right);
            
            Integer index = chars[r];
            if(index != null && index>=left && index<right) left = index+1;
            
            result = Math.max(result, right-left+1);
            chars[r] = right;
            right++;
            
        }
        return result;
    }
```
We change the direct access table from int array to Integer array, by default the former is 0 and the latter is null. If there was a previous visit to the location in Integer Array, the value will not be null, which mean there is a duplicate.

The key understanding is that the integer array will store the index of the character i.e. a = 65 in ascii, chars[a] = chars[6] -> 0. This means that a corresponds to at the position of   65 in the array and the value stored is 0. This creates a mapping.

To clearify, we get the right pointer's character and use this character to access the index corresponding to this character in the integer array. If the inde is not null, it implies that there was a previous visit to this location meaning a duplicate in the substring, and the index of the duplicated character is stored in the integer array, and we increment the index by 1 to contract directly to the targeted position. Then we find the maximum between previous best and the current window right-left+1. Finally, set the index of right for the charcter at the position corresponding to right and increment right.



Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.
Note that you must do this in-place without making a copy of the array.

Movezero inplace

```
class Solution {
    public void moveZeroes(int[] nums) {
       
       for(int i=0, j=0; i<nums.length; i++)
       {
           if(nums[i] != 0)
           {
               int tmp = nums[i];
               nums[i] = nums[j];
               nums[j] = tmp;
	       j++;
           }
       }
    }
```

The code maintain the followign invariant
1. All elements before the slow pointer (lastnonZeroFoundAt) are non-zeroes
2. All eleemnts vetween the current and slow pointer are zeroes.

**Explanation**
We keep two pointers, one recording new non-zero element and the slow pointer record the position of a zero element. Whenever a nonzero element   were found, swap the element of both pointers. The slow pointer will always points to a zero element before swaping and points to the lost non zero element in the aray

This question is under the category of "array transormation". This categoru is very common in tech interview because it's easy and simle to traverse and represent.

The range of integer of 32-bit integer range is -2^32 to (2^32) -1. When working with integer we need to bear in mind that the range of integer value.

we need to first check appending the digit is safe or not; otherwise handle the underflow     or overflow.

For our checking condition we need to check if the result is smaller than INT_MAX /10 beause we are going to multiply 10 to the result.

There are three cases for integer greater overflow and underflow follow similarly
1. The resutl is less than INT_MAX = 2^32 / 10, then we are free to add any digit
2. The result is larger than INT_MAX / 10, then we return INT_MAX
3. The resutl is equal to INT__MAX / 10, then we further check if the digit is smaller or euqal  INT_MAX mod 10; otherwise it will flow.

For cases one and two, overflow and underflow are the same, but the third case for overflow the digit range is 0-7 wherease underflow the digit range is 0-8 because positive range is 2^32 -1.

Complexity analysis:
IF N is the number of character in the input string
* Time complexity O(N)
* Space complexity O(1)



We need to implement a function that converts the given string into a signed 32-bit integer.
Intuitively, we could build the output number out of the input string by iterating over it character by character. However, we stop building the number when a non-digit character is spotted, or the number becomes too large to fit inside a 32-bit signed integer. In the latter case, we need to clamp the result to fit the range.

We will build the integer one character at a time. As we traverse the string from left to right, for each digit character, we will shift all digits in the current integer to the left by one place (this is done by multiplying the integer by 10). Then, we can simply add the current digit to the unit place of the integer. To better understand how this process works, let's look at an example:
The range of integer of 32-bit integer range is -2^32 to (2^32) -1. When working with integer we need to bear in mind that the range of integer value.

we need to first check appending the digit is safe or not; otherwise handle the underflow     or overflow.

For our checking condition we need to check if the result is smaller than INT_MAX /10 beause we are going to multiply 10 to the result.

There are three cases for integer greater overflow and underflow follow similarly
1. The resutl is less than INT_MAX = 2^32 / 10, then we are free to add any digit
2. The result is larger than INT_MAX / 10, then we return INT_MAX
3. The resutl is equal to INT__MAX / 10, then we further check if the digit is smaller or euqal  INT_MAX mod 10; otherwise it will flow.

For cases one and two, overflow and underflow are the same, but the third case for overflow the digit range is 0-7 wherease underflow the digit range is 0-8 because positive range is 2^32 -1.

Complexity analysis:
IF N is the number of character in the input string
* Time complexity O(N)
* Space complexity O(1)

```
class Solution {
    public int myAtoi(String s) 
    {   
      
        int sign = 1;
        int result = 0;
        int index = 0;
        int n = s.length();
        
        //discard all spaces from the beginnign of the input string
        while(index < n && s.charAt(index)==' ')
            index++;
        
        //sign = +1 , if it's positive number, otherwise sign = -1
        if(index < n && s.charAt(index)== '-')
        {
            sign = -1;
            index++;
        }else if(index<n && s.charAt(index) == '+')
        {
            sign = 1;
            index++;
        }
        
        //raverse next digits of input and stop if it is not  digit
        while(index<n && Character.isDigit(s.charAt(index))){
            
            //this is to convert the value to the normal integer value, because in ascii                //zero is 48 and the digit might larger than the value
            int digit = s.charAt(index) - '0';  
            
            //check overflow and underflow conditions
            if((result > Integer.MAX_VALUE / 10) || 
               (result == Integer.MAX_VALUE/ 10 && digit > Integer.MAX_VALUE % 10))
            {
                //handle      under or over  flow depeding on the sign.
                return sign == 1 ? Integer.MAX_VALUE :Integer.MIN_VALUE;
            }
            
            //append current digit to the result
            
            result = 10 * result  + digit;
            index++;
        }
        
        return sign*result;
    }
      
}
```

##### Add two Binary Number
Given two binary strings a and b, return their sum as a binary string.

```
class Solution {
    public String addBinary(String a, String b) {
       int n = a.length();
        int m = b.length();
        if(n<m)
            return addBinary(b,a);
        
        int L = Math.max(n,m);
        
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        int j = m-1;
        
        for(int i=L-1; i>-1; --i)
        {
            if(a.charAt(i) == '1')
            {
                ++carry;
            }
                
            if(j > -1 && b.charAt(j--)=='1')
            {
                
                ++carry;
            }
            if(carry % 2 == 1) 
            {
                sb.append('1');
            }
            else{
                sb.append('0');
            }
            
            carry /= 2;
        }
        
        if(carry == 1)
        {
            sb.append('1');
        }
        sb.reverse();
        
        return sb.toString();
    }
}
```
**Explantion**
This method for adding two binary number and return the binary representation firstly gets the 
length of string b and string a. We get the max length of a and b. Then we create a string builder.
Thirdly, we create an int carry of 0 and an int j of m-1;

We loop through two binary string starting from the right. If the first string at position i is 1 then
we increment the carry(进位）, and then we are assuming a is longer than b, so we need to check if j is larger than -1, i.e. index not out of bound, and if string b at jth position equal 2 then we increment carry by one. After that we check if carry mod 2 is equal to 1, then we append a char 1 to the end of string builder; otherwise, we append char 0 to the end of string builder. After single iteration divide carry by 2.

Finally, we check the final result of carry equal to 1 that is the final carry if there is carry from previous addition, and then reverse the string because we started from the end. 

Bit Manipulation:
**Note**
1. and & gives the smaller value of two values
2. or | gives the larger value of two values
3. xor ^ exlusive or gives the difference of two values
4. leftshift << multiply the value by two
5. rightshift >> divide the value by two

```
import java.math.BigInteger;
class Solution {
  public String addBinary(String a, String b) {
    BigInteger x = new BigInteger(a, 2);
    BigInteger y = new BigInteger(b, 2);
    BigInteger zero = new BigInteger("0", 2);
    BigInteger carry, answer;
    while (y.compareTo(zero) != 0) {
      answer = x.xor(y);
      carry = x.and(y).shiftLeft(1);
      x = answer;
      y = carry;
    }
    return x.toString(2);
  }
}
```
For addding two integers, we start by creating bigintegers of the value in bianry form, we can do in other way but I don't know yet, and create a binary zero in big integer as well as carry and answer.

We use a while loop to process and set the compariom of binary y with binary zero, if they are not equal. This means that while there is still a carry we continue to make bit manipulation. The xor (exlusive or) calculate the difference between two values which will be used as the answer, and the (x.and(y)).shiftLeft(1) find the carry required. We need to remember that the carry is calculated by (x&y) << 1.


#### Two sums
```
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.
```
```
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int[] result = new int[2];
         for(int i=0; i<nums.length; i++)
        {
            int index = nums[i];
            int find = target - index;
            
            for(int j=i+1; j<nums.length; j++)
            {
                if(nums[j] == find)
                {
                    
                    result[0] = i;
                    result[1] = j;
                }else{
                    map.put(nums[j], j);
                }
            }
        }
        return result;
    }
```
This method is brute force method with time compelxity O(n^2) and space complexity O(n), which is bad if we increate the space complexity. 

```
public int[] twoSum(int[] nums, int target) {
	HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
	
	for(int i=0; i<nums.length; i++){
		int diff = target - nums[i];
		if(map.containsKey(diff)){
			return new int[]{map.get(diff, i};
		}else{
			map.put(nums[i], i);
		}
	}
	return null;
}
```
We create a hashmap contains map integer to integer. The former is the element in the array and the latter is the index of the elment in the array.
We loop through the array and fix the element at position i and check if the difference = target - nums[i] is contained in the key. If it is contained, then
we get the index of the difference in the map and return that with index i in an array; otherwise, we put the the element and its index into the map.
THe hashmap technique helps us to remember what we have encountered.




Remove Duplicate from sorted string
```
class Solution {
    public int removeDuplicates(int[] nums) {
        int index = 0;
        for(int i=1; i<nums.length; i++)
        {
            if(nums[i] != nums[index])
            {
                index++;
                nums[index] = nums[i];
          
            }
        }
        return ++index;
    }
}
```
For removing duplicate, I thought the idea of two pointers. One extends for new element and the other keep track of the unique index.
We create two pointer one index and the other i, starting from 0 and 1 respectively. i starts from 1 because nums[index]] will equal, and this is useless.
In the for loop we check if element in nums at ith position duplicate with the value at index position. If not equal, we increment index by one, which index will
be the place the non-duplicate value will be stored.



Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.
```
import java.util.HashMap;
class Solution {
    static HashMap<String,Integer> map = new HashMap<String,Integer>();
    
    static{
                map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);
    }
    public int romanToInt(String s) {
        
        String lastSymbol = s.substring(s.length()-1);
        int lastValue = map.get(lastSymbol);
        int total = lastValue;;
        
        for(int i = s.length()-2; i>=0; i--)
        {
            String currentSymbol = s.substring(i, i+1);
            int currentValue = map.get(currentSymbol);
            if(currentValue < lastValue)
            {
                total = total - currentValue;
            }else{
                total += currentValue;
            }
            lastValue = currentValue;
        }
        
        
        return total;
    }
}
```

Remove Element
> Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The relative order of the elements may be changed. Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements. Return k after placing the final result in the first k slots of nums. Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

```
class Solution {
    public int removeElement(int[] nums, int val) {
        int index = 0;
        for(int i=0; i<nums.length; i++)
        {
            if(nums[i] != val)
            {
                int tmp = nums[i];
                nums[i] = nums[index];
                nums[index]  = tmp;
                index++;
            }
        }
        return index;
    }
}
```
Remove element question in array is similar to move zero question finished ealier. We use two pointers one extends new element and another keep record of the
non targeted string. Start the index from 0 rather copy from other by -1. Use your own style and logic. When we don't find the targeted value we swap with
the index pointer pointing to the targeted value val. nums[i] will be non-targeted value and we can swap them.


Search insert Position

>Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with O(log n) runtime complexity.

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int index, left = 0;
        int right = nums.length-1;
        while(left<=right)
        {
            index = left + (right-left)/2;
            if(nums[index] == target) 
                return index;
            
            if(nums[index] < target) 
                left = index + 1;
            else
                right = index -1 ;
            
        }
        return left;
    }
}
```
Since the array is sorted, this is a good sign of binary search. We record three values in the array, left, right, and index(the middle point)
The binary search is used in the while loop, we find the middle point of left and right. In this code, we concern about the situation that the array out of bound,
so we use left + (right-left)/2. This finds the middle point also without overflow. Other things just follow the binary search and if the while loop is broken,
then the value is not in nums. Here we make left to be index + 1 when nums[index] < target, and make right to be index -1 when nums[index] > target. 

The pointer left is the final position to be inseretd if the value were not in the array. 



Maximum subarray

>Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum. A subarray is a contiguous part of an array.
```
class Solution {
    public int maxSubArray(int[] nums) {
        int current_sum = nums[0];
        int max_sum = nums[0];
        
        for(int i=1; i<nums.length; i++)
        {
            int current_val = nums[i];
            
            current_sum = Math.max(current_val, current_sum + current_val);
            max_sum = Math.max(current_sum, max_sum);
        }
        return max_sum;
    }
}
```
We create two   variables, current_sum and max_sum which were instantiated by the first value of nums. We loop through the array starting from the second element
The element at ith position in nums was instantiated as current_val. We are trying to check whether adding the current_val to current_sum the overall sum will be 
larger or not. We take the current_sum as the max between current_val and current_sum + current_val. Morever, the max_sum will be the max between the current_sum
and the max_sum.


Plus one
>You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's. Increment the large integer by one and return the resulting array of digits.

```
class Solution {
    public int[] plusOne(int[] digits) {
        
        int n = digits.length;
        for(int i=n-1; i>=0; i--)
        {
            if(digits[i] == 9)
            {
                digits[i] = 0;
            }else{
                digits[i]++;
                return digits;
            }
        }
        
        digits = new int[digits.length+1];
        digits[0] = 1;
        return digits;
    }
}
```
I came up the for loop part, but the part that instantiatign a new array for digits and set the beginning to 1 was not thought. Instantiating a new array for int
by default all element were set to zero, and we set the first place to be one , which is what was expected. We reach the end of the for loop without the return
this means that all elements were set to zero and the else part was not triggered. This is the reason we can do the instantiation. 
