# ARC-workflow

Described below is the workflow we performed to determine AMR genes from Enterococcus spp. isolates


## Next Gen Illumina Sequencing

DNA sequencing was done through next gen Illumina sequencing. In summary:
1. DNA was fragmented and adapters were added
2. DNA anchored via capture sequences on adapters (added to both sides), with one side binding to flow cell anchor
3. Bridge amplifcation makes a cluster of thousands of copies of the same DNA fragment
5. Flourescently tagged bases are added one by one, average flourescent signal is detected for each cluster.


Dual Index
![Illumina paired end summary.png](NGS%20sequencing/Illumina%20paired%20end%20summary.png)

## Initial quality check
Although it is important to trim the reads to get a more accurate representation of the final genome (removing low quality reads, removing adapters), it is also a good practise to check the quality of the reads prior to trimming, to have a baseline to compare the trims to.
### percent adapter content
![adapter_content_initial.png](multiQC%20before%20trimming/fastqc_adapter_content_plot.png)

Note the number the percentage of sequences being at yellow/red zones for adapter content, indicating that adapters have not been trimmed  off yet (which is to be expected)

### Over represented sequences:
![overepresented_sequence_initial.png](multiQC%20before%20trimming/over_represented_sequences_pretrim.png)
This shows the sequences that are represented in the reads more often than should be normal in typical library.
It is possible that these sequences are caused by leftover DNA fragments from the sequencing step, highlighting the importance of a rigorous trimming procedure.
Note the poly-G repeated sequence, which is repeated much more often than the other repeats. We presently have no clear explenation for this phenomenon, and we will attempt to remove some of these reads by considering the poly-G tail as an adapter.



## Overall quality
![quality_pretrim](multiQC%20before%20trimming/fastqc_per_base_sequence_quality_plot_pretrim.png)

The pretrim quality score is very good, indicating that the detected bases are highly likely to be the real bases within the genomes that were sequenced. Note the dropoff in quality at larger sequence lenghts, this is caused by DNA primer losing effectiveness at longer lenghts, and can be alleviated by eliminating lower quality reads through trimming.
     


## trimming step (cutadapt → eautils):

It is standard practise to use trimming software to remove any vestiges from the sequencing procedure, such adapter sequences that are used as part of the sequencing procedure. The software also removes lower quality (phred) reads, giving higher overall quality reads, and therefore more accurate data.

We initially used cutadapt but found that some adapter sequences were still being detected by fastqc, we switched to eautils tool as part of the mcf suite, and inputted a custom adapter list that was commonly used within the vetmed department. Critically, we manually inputted a new "adapter" that consisted of the poly-G sequence that was detected by multiqc. While a bit more manual than we preffered, we concluded that the poly-G tail is likely some sort of artifact that is not indicative of the actual genome, so removing as much of it as possible was critical to obtain the most accurate reads possible

### second quality check (cutadapt: prior to eautils)

#### adapter content
![adapter_content_initial.png](multiQC%20during%20%2B%20after%20trimming/adapter_content_initial.png)

Following trimming, adapter content did decrease as expected. Notably, only half of the adapters were actually trimmed by cutadapt.This could be because we trimmed without the use of an adapter library, so the software may have been unable to detect all the adapters.

#### Over represented sequences:

![overepresented_sequence_initial.png](multiQC%20during%20%2B%20after%20trimming/overepresented_sequence_initial.png)
The overrepresented poly-G repeated sequence did decrease in quantity, although it is still by far the most represented sequence.
For that reason, we chose to manually input a poly-G sequence as an "adapter" in an attempt to further decrease the quantity of the repeated sequence, once we ran a second trim.

#### quality score
![quality_score_final_trim.png](multiQC%20during%20%2B%20after%20trimming/quality_score_final_trim.png)

the quality improved further downstream, this is very common following trimming as the better quality reads are selected.

### third quality check (after eautils)
Using the eautils program within the mcf suite, we added a list of adapters that we use for illumina sequences, as well as our poly-G tail


### adapter content:
![adapter_content_final.png](multiQC%20during%20%2B%20after%20trimming/adapter_content_final.png)
Finally, multiqc was not able to detect a high amount of adapter sequences, suggesting that our adapter sequences were able to be properly trimmed from our reads


### Over represented sequences:
![overrepresented_sequences_final.png](multiQC%20during%20%2B%20after%20trimming/overrepresented_sequences_final.png)
The number of poly-G tails that were detected did decrease, but not by much. It is worth noting that fastqc is more sensitive, while triming programs are more selective in what they chose to trim.
The reason for this is simple, we want to try to avoid removing actual sequences, while quality checking however, we want to make sure we are detecting any possible errors.
Furthermore, we though it highly unlikely that the poly-G tails would lead to the detection of AMR genes that are not there. While it may lead to less accurate genomes if they are incorporated into contigs, our focus is on antibiotic resistance genes, therefore we decided to proceed, knowing that some poly-G tails remain in our genomes.

### quality score:
![quality_score_final_trim.png](multiQC%20during%20%2B%20after%20trimming/quality_score_final_trim.png)
our quality score improved marginally, this is caused by the slightly different cutoff values between eautils and cutadapt. 

It is worth noting that the quality score refers to the certainty that a flouresent signal that was detected corresponded to the wavelength of light that flourophores on ddNTPs emit, therefore, this change is not too impactful. Our quality score was already high, and we wanted to trim again to remove a poly-G sequence, with high certainty, this won't affect the poly-G tail very much














#### intial trim with cutadapt: no adapters given







