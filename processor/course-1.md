# From DNA to Proteins. A well explained computational Process.


#### Step 1: findStartCodon

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


Next, we will implement the findStop codon function given that a start codon was found. Remember, a gene must have a start codon. Therefore, there will be no need to find the stopcodon given no start codon. This also means that in case a start codon was found, we can start searching for a stopcodon after the start codon location.  

#### Step 2: findStopCodon version 1

```python
def findStopCodon(dna, startIndex,stopCodon):
    dna = dna.upper()
    dna_length = len(dna)
    stopIndex = None
    
    for i in range(startIndex + 3,dna_length-3):
        if dna[i:i+3] == stopCodon:
            stopIndex = i
            break
                
    return stopIndex
    
```

Finding a start and a stop codon cannot guarranttee the presence of a gene. The sequnce of characters starting with a start codon and ending with a stop codon must be a multiple of three. We will now modify our function to satisfy this condition.   

#### Step 3: findStopCodon version 2  

```python
def findStopCodon(dna, startIndex,stopCodon):
    dna = dna.upper()
    dna_length = len(dna)
    stopIndex = None
    
    for i in range(startIndex + 3,dna_length-3):
        if dna[i:i+3] == stopCodon:
            if (stopIndex - startIndex) % 3 == 0:
                stopIndex = i
                break
                
    return stopIndex
    
```

#### Step 4: find_Stop_Index version 1

```python
def find_Stop_Index(DNA, startIndex):
    stopCodons = ["TAA", "TAG", "TGA"]
    stopIndices = []
    
    for stopCodon in stopCodons:
        stopIndex = findStopIndex(DNA, startIndex, stopCodon)
        stopIndices.append(stopIndex)
    
    return stopIndices
    
```

The function find_stop_Indices returns a list of stop indices. But we only need one stop index at a time. And is the index that occurs before the others. If no index was found the stopIndices would be a list of Nones. Then we can just select one None.

#### Step 5: find_Stop_Index version 2  

```python
def find_Stop_Index(DNA, startIndex):
    stopCodons = ["TAA", "TAG", "TGA"]
    stopIndices = []
    
    for stopCodon in stopCodons:
        stopIndex = findStopIndex(DNA, startIndex, stopCodon)
        stopIndices.append(stopIndex)
    
    FinalStopIndex = None
    for stopIndex in stopIndices:
        if stopIndex == None:
            StopIndices.remove(stopIndex)
    if stopIndices != []:
        FinalStopIndex = min(stopIndices)
        
    return FinalStopIndex
    
```

#### Step 6: findGene 

```python
def findGene(dna):
    dna.upper()
    startIndex = findStartCodon(dna)
  
    if startIndex == None:
        return "No gene"
        
    stopCodon = find_Stop_Codon(DNA,startIndex)    
    if stopIndex == None:
        return "No gene"
    else:
        gene = dna[start_Index : stopIndex+3]
   
    return gene
    
```  

What findGene does is that it finds the first gene in a dna string, if any. But in real applications, dna often contain more than one gene. In fact, many genes and we are often interested in finding all the genes. To achieve this, we will define a new function called findAllGenes to find all genes in a given DNA molecule.  

#### Step 7: findAllGenes 


```python
def findAllGenes(dna):
    dna_length = len(dna)
    current_start_Index = 0
    genes = []
    gene_locations = []
    
    for ii in range(dna_length-6):
        current_gene = findGene(dna, startIndex)
        if current_gene != "No gene":
            genes.append(current_gene)
            gene_locations.append(i)
            
    result = (genes, gene_locations)
    return results
```

#### Step 8: DNAStatistics

```python
def DNAStatistics(DNA)
    statistics = {}
    genes, gene_locations = findAllgenes(DNA)
    min_length = len(DNA)
    max_length = 0
    longest_gene = ""
    shortest_gene = ""
    for gene in genes:
        curr_length = len(gene)
        if curr_length > max_length:
            max_length = curr_length
            longest_gene = gene
        if curr_length < min_length:
            min_length = curr_length
            shortest_gene = gene
    
    statistics = {"Number of genes" : len(genes), "Number of nested genes" : , "Nested Genes" : {"Nested Gene Locations" : , "Number daughter genes" : }, "Longest gene" : {"Gene length" : max_length, "Location of longest gene": gene_locations[genes.find(longest_gene)}, "Shortest gene" : {"Gene length" : min_length, "Location of shotest gene": gene_locations[genes.find(shortest_gene)}}
return statistics
```
