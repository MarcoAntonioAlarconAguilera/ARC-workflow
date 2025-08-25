# ARC-workflow

Described below is the workflow we performed to determine AMR genes from Enterococcus spp. isolates

## Genomic Libary prep
<Finish Later>

## Next Gen Illumina Sequencing

DNA sequencing was done through next gen Illumina sequencing. In summary:
1. DNA was fragmented and adapters were added
2. DNA anchored via capture sequences on adapters (added to both sides), with one side binding to flow cell anchor
3. Bridge amplifcation makes a cluster of thousands of copies of the same DNA fragment
5. Flourescently tagged bases are added one by one, average flourescent signal is detected for each cluster.


Dual Index
<img width="6000" height="4200" alt="Illumina paired end summary" src="https://github.com/user-attachments/assets/ee13ebda-de43-4b3e-adae-4a4f3f5e80c3" />




## eautils mcf + cutadapt:

We had intially found problems in our FastQ sequence having too many primers known as
