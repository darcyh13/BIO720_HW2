---
title: "In class assignment week 2, part 2. A worked example using control flow (for loops, if statements, etc)"
author: "Ian Dworkin"
output: 
  html_document:
    keep_md: yes
    number_sections: yes
---

# Introduction
Let's do a little exercise integrating some of the things we have learned. Here are some Illumina HiSeq reads for one of our recent projects:


```r
read_1 <- "CGCGCAGTAGGGCACATGCCAGGTGTCCGCCACTTGGTGGGCACACAGCCGATGACGAACGGGCTCCTTGACTATAATCTGACCCGTTTGCGTTTGGGTGACCAGGGAGAACTGGTGCTCCTGC"

read_2 <- "AAAAAGCCAACCGAGAAATCCGCCAAGCCTGGCGACAAGAAGCCAGAGCAGAAGAAGACTGCTGCGGCTCCCGCTGCCGGCAAGAAGGAGGCTGCTCCCTCGGCTGCCAAGCCAGCTGCCGCTG"

read_3  <- "CAGCACGGACTGGGGCTTCTTGCCGGCGAGGACCTTCTTCTTGGCATCCTTGCTCTTGGCCTTGGCGGCCGCGGTCGTCTTTACGGCCGCGGGCTTCTTGGCAGCAGCACCGGCGGTCGCTGGC"
```

Question 1. what species are these sequences from?

drosophila melanogaster

Question 2. Put all three of these reads into a single object (a vector).  What class will the vector `reads` be? Check to make sure! How many characters are in each read (and why does `length()` not give you what you want.. try...)

Class of vector should be "character". This is confirmed used class(). There is 124 characters in each read. length() does not work because it gives how many elements are in vector, while nchar() will give how many characters are in each element. 


Question 3. Say we wanted to print each character (not the full string) from read_1, how do we do this using a for loop? You may wish to look at a function like `strsplit()` to accomplish this (there are other ways.)

Replace the blanks.

```r
read_1_split <- strsplit(read_1, split = "" , fixed = T) 
```

Question 4. What kind of object does this return? How might we make it a character vector again?

Returns a "list". 


```r
read_1_split <- as.character(unlist(read_1_split))
```
Question 5. How about if we wanted the number of occurrences of each base? Or better yet, their frequencies? You could write a loop, but I suggest looking at the help for the `table()` function... Also keep in mind that for for most objects `length()` tells you how many elements there are in a vector. For lists use `lengths()` (so you can either do this on a character vector or a list, your choice)


```r
table(read_1_split)/length(read_1_split)
```

```
## read_1_split
##         A         C         G         T 
## 0.1854839 0.2822581 0.3225806 0.2096774
```

Question 6. How would you make this into a nice looking function that can work on either  a list or vectors of characters? (Still just for a single read)

```r
nicefunction <- function(x) {
if (mode(x) == "list") {
table(x)/lengths(x)
} else 
  table(x)/length(x)
}
```

Question 7. Now how can you modify your approach to do it for an arbitrary numbers of reads? You could use a loop or use one of the apply like functions (which one)?



Question 8. Can you revise your function so that it can handle the input of either a string as a single multicharacter vector, **or** a vector of individual characters **or** a list? Try it out with the vector of three sequence reads (`reads`).

```r
reallynicefunction <- function(x) {
if(length(x) ==1 & mode(x) == "character"){
x <- strsplit(x, split ="", fixed = T)
x <- as.character(unlist(x))
}
if (mode(x) == "list") {
table(x)/lengths(x)
} else 
  table(x)/length(x)
}

sapply(read_2, reallynicefunction)
```

```
##   AAAAAGCCAACCGAGAAATCCGCCAAGCCTGGCGACAAGAAGCCAGAGCAGAAGAAGACTGCTGCGGCTCCCGCTGCCGGCAAGAAGGAGGCTGCTCCCTCGGCTGCCAAGCCAGCTGCCGCTG
## A                                                                                                                   0.27419355
## C                                                                                                                   0.33064516
## G                                                                                                                   0.29838710
## T                                                                                                                   0.09677419
```
