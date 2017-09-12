---
layout: post
title: Bit Maniputation Summary
date: 2017-09-10
categories: 
  - Algorithm
tags: 
  - algorithm
  - java
  - programming
---

## 1. Concept
Wikipedia:
> Bit manipulation is the act of algorithmically manipulating bits or other pieces of data shorter than a word. Computer programming tasks that require bit manipulation include low-level device control, error detection and correction algorithms, data compression, encryption algorithms, and optimization. For most other tasks, modern programming languages allow the programmer to work directly with abstractions instead of bits that represent those abstractions. Source code that does bit manipulation makes use of the bitwise operations: AND, OR, XOR, NOT, and bit shifts.

Bit manipulation includes & (and), | (or), ~ (not) and ^ (exclusive-or, xor) and shift operators a << x and a >> x.

## 2. Examples

### 2.1 Basics

* Remove the last 1 bit:  n&(n-1)
E.g. n=10 => 1010 & 1001 = 1000

* Get all 1 bits: ~0 or 0xffffffff

* Check even or odd: n&1==1 then odd else even 

### 2.2 Number of 1 Bits

Task: Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight). For example, the 32-bit integer '11' has binary representation 00000000000000000000000000001011, so the function should return 3.<br>

Idea: <br>
1. Use a for loop to check each bit. This way will loop 32 times.
2. Remove the last 1 bit until n is 0. The worst case is 
3. Use defined function Integer.bitCount(int)

Idea 1:
```
    public int hammingWeight(int n) {
        int count = 0;
        for(int i=0; i<32 && n!=0; i++) {
            if((n&1)==1) count++;
            n >>>= 1;
        }
        return count;
    }
```
Idea 2:
```
    public int hammingWeight(int n) {
        int count = 0;
        while(n!=0) {
            n = n&(n-1); // remove the last 1 bit
            count++;
        }
        return count;
    }
```
Idea 3:
```
    public int hammingWeight(int n) {
        return Integer.bitCount(n);
    }
```
Attention: The function Integer.bitCount(int) returns the number of one-bits in complement binary representation. The implement is as follows:
```
    public static int bitCount(int i) {
        // HD, Figure 5-2
        i = i - ((i >>> 1) & 0x55555555);
        i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
        i = (i + (i >>> 4)) & 0x0f0f0f0f;
        i = i + (i >>> 8);
        i = i + (i >>> 16);
        return i & 0x3f;
    }
```

### 2.3 Is power of four

Task: Check whether the input number is power of four
```
    public boolean isPowerOfFour(int n) {
        return (n&(n-1))==0 && (n&0x55555555)!=0;
    }
```
(n&(n-1))==0 : <br>
The number power of four should only have one 1 bit. <br>
E.g. 1=1, 4=100, 16=10000. If the 1 bit is removed, it should be 0. <br>

(n&0x55555555)!=0 : <br>
0x55555555 is 1010101010101010101010101010101 in complement binary representation. <br>
n&0x55555555 defines the place where the 1 bit should appear. <br>


### 2.4 Sum of two integers
    
Task: Calculate the sum of two integers a and b, but you are not allowed to use the operator + and - <br>

Idea: Use ^ and & to add two integers.
```
    public int getSum(int a, int b) {
        return b==0? a : getSum(a^b, (a&b)<<1);
    }
```
a^b : a+b without carry <br>
a&b : a+b with carry <br>
Terminating condition: no carry in any bit, then a+b is equal to a&b.  <br>

### 2.5 Single Number

Task: Given an array of integers, every element appears twice except for one. Find that single one. <br>

Idea: Use ^ to remove numbers appear even times.
```
    public int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums) {
            res ^= num;
        }
        return res;
    }
```
### 2.6 Largest power of 2

Task: Find the largest power of 2 which is less than or equal to the given number N.
```
    public int largestpower(int n) {
        //changing all right side bits to 1.
        n = n | (n>>1);
        n = n | (n>>2);
        n = n | (n>>4);
        n = n | (n>>8);
        n = n | (n>>16); 
        return (n+1)>>1;
    }
```
The left shift is to fill 1 after the most significant bit. That is, 1001 will become 1111.

### 2.7 Reverse bits

Task: Reverse bits of a given 32 bits unsigned integer.
```
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int mask = 1<<31, res = 0;
        for(int i = 0; i<32; i++) {
            if((n&1)==1) res |= mask;
            mask >>>= 1; // 100...000 => 010...000 => 001...000
            n >>>= 1;
        }
        return res;
    }
```
### 2.8 Repeated DNA Sequences

Task: All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
For example, given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT", return ["AAAAACCCCC", "CCCCCAAAAA"].

```
    public List<String> findRepeatedDnaSequences(String s) {
        final int N = s.length();
        final int RANGE = 10;
        List<String> res = new ArrayList<>();
        Set<Integer> wordSet = new HashSet<>(N*4/3);
        Set<Integer> dupSet = new HashSet<>();
        int[] map = new int[26];
        map['A'-'A'] = 0;
        map['C'-'A'] = 1;
        map['G'-'A'] = 2;
        map['T'-'A'] = 3;
        
        for(int i=0; i<N-RANGE; i++){
            int hash = 0;
            for(int j=i; j<i+RANGE; j++){
                hash <<= 2; // need 2 bits to representing A, C, G, T 
                hash |= map[s.charAt(j)-'A'];
            }
            if(!wordSet.add(hash) && dupSet.add(hash)){
                res.add(s.substring(i, i+RANGE));
            }
        }
        return res;
    }
```

## Reference
1. Wikipedia: [Bit manipulation](https://en.wikipedia.org/wiki/Bit_manipulation)
2. Leetcode: [Leetcode](https://leetcode.com/)

