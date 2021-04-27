
# Using Python to find all genes in DNA including nested genes.


We now live in a society where many advanced technologies are being produced every year. Some of these adavnced technologies are DNA sequencing machines. These machines take in purified and amplified DNA and produce a sequence of ACGT characters. Each character in the sequence represents a specific nucleotide. So, what exactly do we do with these DNA sequences? We need to extract important information such as the number genes in a specific location, the replication origin, the type of genes and so on. 

In this post, I will show you a step by step approach on how to find all genes, including nested genes, in a DNA string. We will define a python function that searches through a DNA string for a gene, pinpoint the location of the gene in the DNA string and returns information such as the number of genes, the longest gene, the shortest gene and so on.

First, we will start by defining some molecular biology terminologies. Then, identify some functions. Finally, we will build up our function in a step by step manner. 

It should be noted that I am not aiming to build a very effective function, but rather to build something that is easy to understand. 

The code are documented, but if you want to practice along, you should be familiar with python syntax including data structures like list, strings and functions.

If you are new in python and you wanna get started with some simple biological application, then this post is for. The code that will be writen on this page are not meant to accurate but rather for ellaboration purposes.

In this article, I will show you how you can use simple python codes to find all genes from a DNA sequence, but first, I will like to define some useful terminologies.

1. Nucleotide: Nucleotides are the building blocks of DNA just like amino acids are the building blocks of proteins. There are only nucleotides that make up DNA: Adenine(A), Guanine(G), Thymine(T) and Cytosine(C).
4. Codon: A codon is a sequence of three nucleotides e.g AAT, GCC, ATG, etc. Each code codes for a specific amino acid, although one amino acid can be coded for by more than one codon.
5. Gene: A gene is the functional unit of DNA that can code for a protein. We will see more about in this article.
6. Start codon: A start codon is the first codon found in a gene. There is only one universal start codon: ATG and it codes for the amino acid Methionine.
7. Stop codon: A stop codon is the last codon in a gene. There are three stop codons so far in mammals. They include; TAA, TAG and TGA.
9. double stranded DNA: Eukaryotic DNA occurs in double stranded form, meaning, each DNA molecule is made up of two strands of nucleotides that bond through hidrogens bonds. 
11. Complementary base pairing: The hydrogens bonds formed between strands of DNA are specific between base-pairs. This is called complementary base pairing. Example, C and G pair with each other while A and T pair with each other. 

Haven defined some useful terms, we will dive into the agenda for today. 

We have defined a gene above in simple biological terms. However, for computational purposes, we need to use a different definition:

_A gene is a sequence of codons starting with a start codon and ending with a stop codon_.

From the above definition, any program we write to find genes is DNA must meet the following criteria
1. have a multiple of three length. Since all codons are made up of 3 nucleotides and genes are mage of codons, then genes are made of triplets of nucleotide.
2. A gene length should be greater than 6. This just theory. Most genes are hundreds of nucleotides long.
3. The first codon must be ATG
4. Last codon must be either TAA or TAG or TGA.

That being said, we are gonna need to write functions that carryout the following task
* check condition 1. We can name the function multiOfThree(meaning multiple of three)
* check condition 2. Condition can be considered in findGene function
* Check condition 3. We will define a function to find the start codon(ATG). Obviously in a case where no start codon is found, we can safely assume there is no gene.We may name our function findStartCodon.
* Check codition 4. Will define a function to find a stop function given that any start function was found. Since we are looking for a gene, the search for a stop codon must been after the location of a start codon.

Given the above criteria, we can define a function to find a gene anywhere along a DNA sequence. We can then call this function is a bigger function to findall all the genes in the DNA sequence.

If you are familiar with python programming language, I will advise you to write the code on your own your own since the problem is already defined. Our codes may also be different but do not forgot to download the dataset and compare results.

```python
def MultOfThree(sequence):
    # This function takes in a sequence(string or list or tuple) and finds if the length of the sequence is a multiple of three.
    # The function returns true if the length of the sequence is a multple of 3 and returns false otherwise.
    result = false
    length = len(sequence)
    if length % 3 == 0:
        result = true
    return result
```  
The code snippet above helps to check that condition 1 above is valid for a gene. N

Next we will define a function to find a start codon as shown below.

```python
def findStartCodon(dna):
    dna = dna.upper()
    startCodon = "ATG"
    dna_length = len(dna)
    startIndex = None
    for i in range(0,dna_length-3):
        if dna[i:i+3] == startCodon:
            startIndex = i
    return startIndex
    
```

Next, we will implement the findStop codon function given that a start codon was found. Remember, a gene must have a start codon. Therefore, there will be no need to find the stopcodon given no start codon. This also means that in case a start codon was found, we can start searching for a stopcodon after the start codon. 

We will start out search afer the startcodon in order not to waste time. Hence, our function will need two paramters: The DNA string, and the index of the start codon returned by findStartCodon.



```python
def findStopCodon(dna, startIndex,stopCodon):
    dna = dna.upper()
    dna_length = len(dna)
    stopIndex = None
    
    for i in range(startIndex + 2,dna_length-3):
        if dna[i:i+3] == stopCodon:
            if (stopIndex - startIndex) % 3 == 0:
                stopIndex = i
                break
                
    return stopIndex
    
```

Finding a start and a stop codon cannot guarranttee the presence of a gence. The sequnce of characters starting with a start codon and ending with a stop codon must meet up with codition 02. 

```python
def findGene(dna):
    dna.upper()
    startIndex = findStartCodon(dna)
  
    if startIndex == None:
        return "No gene"
    
    TAA_Index = findStopCodon(dna, startIndex, "TAA")
    TAG_Index = findStopCodon(dna, startIndex, "TAG")
    TGA_ındex = findStopCodon(dna, startIndex, "TGA")
    
    if TAA_Index == TGA_Index == TAG_Index:
        stopIndex = None
    else:
        stopIndex = min(TAA_Index, TAG_Index, TGA_Index)
        
    if stopIndex == None:
        return "No gene"
    else:
        gene = dna[start_Index : stopIndex+3]
   
    return gene
        
  ```  
    
    
What findGene does is that it finds the first gene in a dna string, if any. But in real applications, dna often contain more than one gene. In fact, many genes and we are often interested in finding all the genes. To achieve, we will modify find gene to find genes from any location with the a dna string, then we will defind a major functio that calls findGene to find all the genes in dna.




The code developed on this page, is based on concepts I learnt in a Java Programming course by Duke University on Coursera. However, I explain the code in python because ı think that python programming language is more applicable to datascience than java.


```python
def findAllGenes(dna):
    dna_length = len(dna)
    current_start_Index = 0
    genes = []
    gene_locations = []
    
    for ii in range(dna_length-5):
        current_gene = findGene(dna, startIndex)
        if current_gene != "No gene":
            genes.append(current_gene)
            gene_locations.append(i)
            
    result = (gene_locations, genes)
```


```python
def countGenes(dna):
        gene_indices, genes = findAllGenes(dna)
        count = len(genes)
        
        return count
        
        
```




    
    




