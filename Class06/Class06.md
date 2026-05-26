# Class 6: R Functions
Reta Ado (PID: A17814740)

- [Background](#background)
- [A first function](#a-first-function)
- [A generate_data() function](#a-generate_data-function)
- [Write a \`generate_protein()
  function](#write-a-generate_protein-function)
- [Are Our peptides “unique in
  nature?”](#are-our-peptides-unique-in-nature)

## Background

All functions in R have at least 3 things:

- a *name* (we pick that and use it to call the function)
- input *arguments* (one or more comma separaed inputs that go inside
  the brackets when we call the function),
- the “body” (the lines of R code that do the work of the function).

> Q1a: Your first version of the function should add two input numbers
> together. For example, add(4, 7) should return 11.

## A first function

Here we will create a function to add some numbers. Let’s call it
`add()`

Input arguments can be either **required** or **optional**. The later
have fall-back default that will be used if the user does not specify
them.

``` r
add <- function(x, y = 0) { 
  x + y 
}
```

Can we use our new function:

``` r
add(10, 100) 
```

    [1] 110

``` r
add(10) 
```

    [1] 10

> Q1b: For you second version, adapt your first function so it can take
> a single input vector or two inputs as before. For example, add(4, 7)
> and add( c(4,7,10) ).

``` r
add <- function(x, y = 0) { 
 sum(x, y) 
} 
```

``` r
add(4, 7) 
```

    [1] 11

``` r
add(c(4,7, 10), ) 
```

    [1] 21

> Q1c: Finally, on your own (outside of class) create a third version of
> your function that can add any number of inputs that the user
> provides. For example, add(1, 2, 3, -4) should return 2. Hint: explore
> the … (dots) argument or the base R function sum().

``` r
add <- function(...) {
  sum(...)
}
```

``` r
add(1, 2, 3, -4)
```

    [1] 2

We can explicitly set a **return** value output from a function (rather
than the default last line) by using the `return()` function call.

``` r
add<- function(x, y=0, z=0) {
  sum(x, y, z) 
  cat("Is it break time yet\n")
  } 

add(10,100) 
```

    Is it break time yet

## A generate_data() function

A useful function here is the “base R” `sample()` function:

``` r
sample(1:5, size=3) 
```

    [1] 5 2 3

We can use this to make a random nucelotide seuence if we draw from “A”,
“C”, “G”, and “T”…

``` r
sample(1:5, size=60, replace = TRUE) 
```

     [1] 1 2 4 5 4 1 5 4 5 4 2 4 1 3 5 1 1 5 5 3 5 3 4 2 1 3 1 2 4 1 5 3 4 4 1 1 1 5
    [39] 1 4 1 5 4 4 1 2 5 1 4 1 5 4 2 1 3 3 4 1 3 4

``` r
sample(x=c("A", "C", "G", "T"), size=10, replace = TRUE) 
```

     [1] "G" "C" "A" "T" "G" "C" "C" "C" "G" "T"

> Q2a: Write a function `generate_dna()` that returns a random DNA
> sequence of a length specified by the user. Your first version should
> return a multi-element vector of single character nucleotides. For
> example generate_dna(6) might return “A”, “T”, “T”, “G”, “A”, “C”.

``` r
generate_dna <- function(len) {
  sample(x=c("A", "C", "G", "T"), size=len, replace = TRUE) 
} 
```

``` r
generate_dna(len=100) 
```

      [1] "T" "C" "A" "G" "C" "T" "G" "G" "A" "C" "G" "T" "A" "A" "G" "T" "C" "G"
     [19] "G" "T" "A" "G" "A" "G" "G" "C" "C" "T" "G" "A" "G" "G" "A" "T" "C" "C"
     [37] "A" "T" "G" "G" "A" "T" "T" "G" "C" "C" "T" "G" "G" "C" "G" "G" "A" "T"
     [55] "T" "A" "G" "A" "G" "A" "A" "C" "T" "T" "C" "T" "C" "A" "C" "A" "G" "A"
     [73] "C" "A" "A" "T" "A" "G" "C" "G" "C" "G" "T" "A" "T" "G" "C" "C" "A" "A"
     [91] "C" "T" "G" "A" "A" "C" "G" "C" "A" "C"

``` r
generate_dna() 
```

    [1] "C" "G" "A" "G"

> Q2b: Your second version should optionally be able to return either a
> multi-element vector of single character nucleotides (as before) or a
> single character string (not a vector of single letters but a singe
> vector of multiple letters). For example “AAGGCTG”.

``` r
generate_dna <- function(len, single.element = FALSE) {
  ans <- sample(x = c("A", "C", "G", "T"), size = len, replace = TRUE)
  
  if (single.element == TRUE) {
    ans <- paste(ans, collapse = "")
  }
  
  return(ans)
}
```

``` r
#example of expected behavior 
generate_dna(6)
```

    [1] "G" "C" "G" "A" "T" "G"

``` r
#example of single character string 
generate_dna(6, single.element = TRUE)
```

    [1] "TCCCAA"

Functions that could be useful here are `paste()`. `if(),`cat(), and
`return()`

``` r
paste(c("A", "C", "G"), collapse = "---" ) 
```

    [1] "A---C---G"

> Q2c: Finally, create a final version of your function that prints out
> a FASTA format sequence with an id line indicating the sequence
> length.

    len9
    CGAAGGCTG

``` r
cat("hello \t there") 
```

    hello    there

``` r
generate_dna <- function(len=10, single.element=TRUE) {
  
  ans<- sample(x=c("A", "C", "G", "T"), size=len, replace = TRUE)
  #cat("Hello....")
  
  if(single.element) {
    #cat("is it me you are looking for...")
    ans <- paste( ans, collapse = "")
  }
  
  ## Format as FASTA with an ID line
  
  cat( paste(">len", len, "\n", sep=""))
  cat(ans)
  cat("\n")
  ##
  
  return(ans)
}
```

``` r
x <- generate_dna(20)
```

    >len20
    CACCATGGATATGCGACTGG

## Write a \`generate_protein() function

> Q3: Write a function generate_protein() that returns a random
> peptide/protein sequence of a length specified by the user. For
> example generate_protein(6) might return “WQRTAG”.

``` r
generate_protein <- function(len) {
  
  # Write out all 20 standard amino acids using single letter code
  aa <- c("A", "R", "N", "D", "C", "E", "Q", "G", "H", "I", "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V") 
  
  # Randomly  sample amino acids with replacement to build a protein sequence
  # size = len means the sequence will be as long as the user specifies
  ans <- sample(x = aa, size = len, replace = TRUE)
  
  # Collapse the vector into a single string 
  paste(ans, collapse = "")
 
} 
```

``` r
# Example of the expected behavior
generate_protein(6) # e.g. "WQRTAG" 
```

    [1] "AMHKPS"

> Q4: Adapt and use your generate_protein() function to generate a
> series of random protein sequences ranging from 6 to 13 amino acids in
> length (one sequence per length). Take advantage of the base R
> function for() or sapply() so that you do not have to call
> generate_protein() eight times by hand.

``` r
for(l in 6:13) { 
  cat(">id.", l, "\n", sep = "") 
  cat(generate_protein(l), "\n")  
  
  
} 
```

    >id.6
    VKLEND 
    >id.7
    KFWSADP 
    >id.8
    TSKMYQRS 
    >id.9
    HTCGADEMP 
    >id.10
    RWWAAGSDPH 
    >id.11
    PRHPMHQTKGP 
    >id.12
    ETHIMQWCAYFL 
    >id.13
    PSSATPNLPVIYT 

``` r
generate_protein(6) 
```

    [1] "SRRTIS"

# Are Our peptides “unique in nature?”

> Q5: Take your FASTA-formatted peptides from Q4 and run them as a
> single BLASTp search against the Non-redundant protein sequences (nr)
> database at https://blast.ncbi.nlm.nih.gov/. For this question do not
> restrict the organism (leave the Organism field blank so that the full
> nr database is searched).

<table>
<thead>
<tr>
<th>Length</th>
<th>Ide</th>
<th>Cov</th>
<th>Uniq</th>
</tr>
</thead>
<tbody>
<tr>
<td>6</td>
<td>100</td>
<td>100</td>
<td>N</td>
</tr>
<tr>
<td>7</td>
<td>100</td>
<td>100</td>
<td>N</td>
</tr>
<tr>
<td>8</td>
<td>100</td>
<td>100</td>
<td>N</td>
</tr>
<tr>
<td>9</td>
<td>100</td>
<td>89</td>
<td>Y</td>
</tr>
<tr>
<td>10</td>
<td>88.89</td>
<td>90</td>
<td>Y</td>
</tr>
<tr>
<td>11</td>
<td>81.82</td>
<td>100</td>
<td>Y</td>
</tr>
<tr>
<td>12</td>
<td>100</td>
<td>75</td>
<td>Y</td>
</tr>
<tr>
<td>13</td>
<td>81.82</td>
<td>85</td>
<td>Y</td>
</tr>
</tbody>
</table>

> Q5a: At which sequence length do your randomly generated peptides
> start to look “unique in nature” (i.e. no 100% coverage + 100%
> identity hit)?

They start to look unique in nature after the 8th sequence length
(starts at 9).

> Q5b: Speculate why very short random peptides are almost always found
> in nr, while longer ones typically are not. Your answer should refer
> both to the size of the sequence space (20𝐿 for a peptide of length 𝐿)
> and to the size of the known protein universe.

Short random peptides are almost always found in nr because their
sequence space (20^L) is very small compared to *n* (number of sequences
in known protein universe), which makes it likely that any randomly
generated sequence has been sampled by a natural protein. As peptide
length (L) increases, the sequence space grows exponentially while the
number of known proteins *n* remains fixed. Once sequence space exceeds
*n*, the vast majority of possible sequences haven’t appeared in any
natural protein, making it increasingly unlikely that a randomly
generated peptide of length L would be found in the database.

> Q6: In 3–6 sentences total and using your Q5 data and the reasoning
> from Q5b, what do you think this minimum length is and why might it be
> a bad design choice for the immune system to present very short
> peptides?

The minimum useful peptide length is around 9 amino acids since this is
where sequences first become unique in nature. Presenting very short
peptides (like 6,7,8 in my example) would be a bad design choice for the
immune system because their sequence space is too small, meaning those
sequences are too short to be distinctive from naturally occurring
proteins. Therefore, the body’s immune system does not have ability to
determine pathogens from self peptides, triggering possibly bad
autoimmune responses. Longer peptides have a larger sequence space
(ex:20^9), making it much more likely that a peptide is unique to a
pathogen rather than shared with a host protein.
