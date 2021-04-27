
# Using Python to find all genes in DNA: A step by step Approach.

We now live in a society where many advanced technologies have been developed. With some of these technologies, genome(DNA) sequencing has become a child's play. DNA sequencing machines take in purified and amplified DNA and produce a sequence of ACGT characters. Each character in the sequence represents a specific nucleotide.

### What exactly do we do with these DNA sequences?  
We need to extract important information such as the number genes in a specific location, the replication origin, the type of genes and so on. 

In this post, I will show you a step by step approach on how to find all genes, including nested genes, in a DNA string. We will define a python function that searches through a DNA string for a gene, pinpoint the location of the gene in the DNA string and returns information such as the number of genes, the longest gene, the shortest gene and so on.

First, we will start by defining some molecular biology terminologies. Second, identify some functions. Then, we will build up our function in a step by step manner. Finally, we will run our final function on the genome of Escherichia coli, a bacteria that lives in the intestines of mammals. 

It should be noted that I am not aiming to build a very effective function, but rather to build something that is easy to understand that you can play with. So, if you are an advanced programmer, don't be shocked if you see some low level programming concept. Also, if you read the post and have any contributions to add, please drop a comment in the comment section. It is based on your comments that I will update this post to suit the my audience needs.  

So, let's get started.

### Some Molecular Biology Terminologies  
1. **Nucleotide:** Nucleotides are the building blocks of DNA just like amino acids are the building blocks of proteins. There are only nucleotides that make up DNA: Adenine(A), Guanine(G), Thymine(T) and Cytosine(C).
2. **Codon:** A codon is a sequence of three nucleotides e.g AAT, GCC, ATG, etc. Each code codes for a specific amino acid, although one amino acid can be coded for by more than one codon.
3. **Gene:** A gene is the functional unit of DNA that can code for a protein. We will see more about in this article.
4. **Start codon:** A start codon is the first codon found in a gene. There is only one universal start codon: ATG and it codes for the amino acid Methionine.
5. **Stop codon:** A stop codon is the last codon in a gene. There are three stop codons so far in mammals. They include; TAA, TAG and TGA.
6. **Double stranded DNA:** Eukaryotic DNA occurs in double stranded form, meaning, each DNA molecule is made up of two strands of nucleotides that bond through hidrogens bonds.
7. **Complementary base pairing:** The hydrogens bonds formed between strands of DNA are specific between base-pairs. This is called complementary base pairing. Example, C and G pair with each other while A and T pair with each other. 
8. **Nested Genes:** These are genes found within other genes. Similar to how DNA contains genes, some genes may contain other genes as well.

We have defined a gene above in simple biological terms. However, for computational purposes, we need to use a different definition:
_A gene is a sequence of codons starting with a start codon and ending with a stop codon_.

We shall define our python code based on the above definition of a gene. Thus, our function must meet the following criteria:  
1. The gene length must be a multiple of three. Since all codons are made up of 3 nucleotides and genes are mage of codons, then genes are made of triplets of nucleotide.
2. A gene length should be greater than 6 with the start and stop codons inclusive. This is just theory though, since most genes are hundreds of nucleotides long.
3. The first codon must be ATG(A start codon)
4. The last codon must be either TAA or TAG or TGA which are all stop codons.

That being said, we are gonna need to write functions that carryout the following task
* check that gene length multiple of three.
* find a start codon. We will define a function to find the start codon(ATG). Obviously in a case where no start codon is found, we can safely assume there is no gene.We may name our function findStartCodon.
* find a stop codon. Will define a function to find a stop codon given that any start codon was found. Since we are looking for a gene, the search for a stop codon must been after the location of a start codon.

Given the above criteria, we will build our function according to the following steps:  

### STEPS
**Step 1 :** Build a function called findStartCodon that takes in a DNA string as parameter and returns the location( or index) of a start codon. If no start codon is found, it returns None.  
**Step 2 :** Build a function called findStopCodon that takes in a DNA string and an integer as parameter(the start index), and returns the position of a stop codon. If no stop codon is found, it returns None.  
**Step 3 :** Modify the findStopCodon function to consider only stop codons that are multiples of three away from the start codon.  
**Step 4 :** Modify the findStopCodon function again, to take in as parameters; DNA string, startIndex and Stop codon( TAG or TAA ot TGA) and search for all the three types of stop codons. This new function will return a list of stop indices.  
**Step 5 :** Modify the findStopCodon function finally, to consider the minimum stop codon incase more than one stop codon is found.   
**Step 6 :** Build a function to find a gene called findGene that takes in as parameters; a DNA string, the start codon and the stop codon.  
**Step 7 :** Build a function to find all genes in a DNA string by making use of the functions previously defined.   
**Step 8 :** Define a function called DNAStatistics that that takes in a DNA string as parameter and finds how many genes are in DNA, the longest gene and length, the shortes gene and length, nested genes.  
**Step 9 :** Run the DNAstatistics function on Escherichia coli genome.  

#### Step 1

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




    The code are documented, but if you want to practice along, you should be familiar with python syntax including data structures like list, strings and functions.
    




