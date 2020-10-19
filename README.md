# MENdel

This repository contains python scripts for running MENdel via a commandline interface. MENdel is a binary classifier that predicts the occurrence of Microhomology-Mediated End Joining (MMEJ) or 1bp-insertion derived single majority outcomes (SMOs) as a result of targeted DNA double strand break repair (DSBR). SMOs are DSBR outcomes where a single sequence is represented at a frequency of 0.50 or higher. MENdel combines the MMEJ-deletion predictions of MENTHU (Mann & Martínez-Gálvez et al., *Nucleic Acids Research*, 2019) and the insertion predictions of Lindel (Chen et al., *Nucleic Acids Research*, 2019) to generate SMO predictions. MENdel is compatible with Linux, Mac, Windows (using Cygwin), and Windows CMD. To run MENdel, it is required that the user has MENTHU-command-line and Lindel cloned locally. Follow the instructions given below to install each of these tools.

```
mkdir MENdel_root
cd MENdel_root
```

1. **MENTHU-command-line installation**
   
   Follow instructions at https://github.com/parnaljoshi/MENTHU-command-line to install the commandline version of MENTHU within the previously created directory MENdel_root. The README file contains instructions to download and install R and necessary R packages to run MENTHU, if not already present.

2. **Lindel installation**

   Lindel is compatible with ```Python2.7``` and ```Python3.5``` or higher. Follow instructions at https://www.python.org/ or https://www.anaconda.com/ for installation.
   
   Follow instructions at https://github.com/shendurelab/Lindel to install Lindel within the directory MENdel_root.

3. **MENdel and required Python packages installation**

   Navigate to the previously created directory MENdel_root and follow these instructions to install scripts to run MENdel.
   
   ```
   git clone https://github.com/parnaljoshi/MENdel/
   cd MENdel
   ```
   
   Alternatively, use the green "Code" button in the upper right corner and choose "Download ZIP"
   
   - For Windows users
   
   ```
   Windows_InstallPackages.bat
   ```
   
   Alternatively, double-click on the .bat file to run it.
   
   - For Linux/Mac/Windows using WSL or Cygwin users
   
   ```
   bash InstallPackages.sh
   ```

4. **Running MENdel**

   MENdel-command-line can be run from Unix-like command lines (Linux, Mac, Cygwin on Windows) or Windows CMD. However, due to end-of-line conversion differences between Unix and DOS for MENTHU, **you must use MENdelScript.py on Linux, Mac, and Cygwin, and Windows_MENdelScript.py in Windows CMD**. 

   Check to make sure you're using the correct command for your system!

   MENdel-command-line can be run using the following syntax:
   
   - For Windows users
   
   ```
   python Windows_MENdelScript.py [-o outFile] [-c CRISPR Option] [-p PAM Sequence] [-d Distance to DSB] [-oh Overhang] [-to TALEN Option] [-ts TALEN scheme] [-g Gen Input Type] [-i Gen Input] [-st Score Threshold] [-t7 T7 opt] [-v verbose] [-va validate]
   ```
   
   - For Linux/Mac/Windows using WSL or Cygwin users
   
   ```
   python MENdelScript.py [-o outFile] [-c CRISPR Option] [-p PAM Sequence] [-d Distance to DSB] [-oh Overhang] [-to TALEN Option] [-ts TALEN scheme] [-g Gen Input Type] [-i Gen Input] [-st Score Threshold] [-t7 T7 opt] [-v verbose] [-va validate]
   ```
   
   "python" tells the system to use ```python``` to execute ```MENdelScript.py``` (or ```Windows_MENdelScript.py``` for Windows CMD).
   
5. **Parameter explanation**

   The parameters are explained below. Each parameter is delimited by a space. Parameter values should not have spaces; if you want to put spaces in the output file name, the name should be in quotes, e.g. "output File.csv". Parameter values (including strings) do not have to be in quotes, except for the output file name exception. All parameters values can be kept to default with the exception of output file name, gen input type, and gen input
   
   |**Parameter        |Accepted Values|Default|Explanation** |
   |-----------------  |---------------|-------|--------------|
   |**outFile**        |Character string followed by extension|MENdel_outfile.csv|The name of the file to output your results to. If using a fasta file with multiple sequences, multiple files will be created, using this as a prefix|
   |**CRISPR Option**  |T or F         |T      |T or F Flags the system to use CRISPR nuclease processing. If this option is T (true) "TALEN Option" must be F (false). **Must be T for MENdel**|
   |**PAM Sequence**   |A PAM sequence |NGG    |The PAM sequence for the CRISPR sequence. Ambiguous nucleotides are allowed. Using N will scan every possible cut site in the target sequence. This parameter must be present, but is not used, if "CRISPR Option" is false (i.e., you can put a 0 or NA in this spot.). **Must be NGG for MENdel**|
   |**Distance to DSB**|Integer      |-3       |The distance from the PAM sequence to the DSB site. For DSBs upstream of a PAM, use a negative value (e.g., -3 for SpCa9); for downstream, use a positive value (e.g., 18 for Cas12a.) This parameter must be present, but is not used, if "CRISPR Option" is false (i.e., you can put a 0 or NA in this spot.)|
   |**Overhang**       |Integer      |0        |Integer >= 0 The length of 5' overhang produced by the nuclease (e.g., 5 for Cas12a). Use 0 for blunt-cutting nucleases. This parameter must be present, but is not used, if "CRISPR Option" is false (i.e., you can put a 0 or NA in this spot.)|
   |**TALEN Option**   |T or F       |F        |Flags the system to use TALEN processing. If this option is T (true), "CRISPR Option" must be F (false). **Must be F for MENdel**|
   |**TALEN Scheme**   |15-18/14 or 16/15-18 or 0/NA|0 |The left arm length, spacer length, and right arm length to use when searching for TALEN locations. E.g., for a TALEN with arms 15 nt long, with spacer 14 nt, use 15/14/15. TALEN arms can be 15-18 nt in length; the spacer should be 14 OR 16 nt in length (15 is not allowed for the spacer). This parameter must be present, but is not used, if "TALEN Option" is false (i.e., you can put a 0 or NA in this spot.). **Must be 0 or NA for MENdel**|
   |**Gen Input type** |gb ens seq file|gb     |Flags the system to get a GenBank/RefSeq ID (gb), Ensembl ID (ens), pasted DNA sequence (seq), or to expect a FASTA file (file)|
   |**Gen Input**      |Accession/Sequence/File|ExampleID|Provide the accession for GenBank/RefSeq/Ensembl inputs, file name for "file" option, or DNA sequence for "seq". If the file name has spaces in it, put this parameter in quotes.|
   |**Score Threshold**|Integer>1    |1.0      |Only output results with MENTHU score above this threshold. Default is 1.0. We recommend to only use sites with score >= 1.5|
   |**T7 Opt**         |T or F       |F        |If T (true), only displays results where the gRNA is compatible with T7-cloning.|
   |**Verbose**        |T or F       |F        |If T (true), outputs progress messages to the console.|
   |**Validate**       |T or F       |F        |If T (true), checks the command line arguments to make sure they are all valid (this may take some time); if F, skip validation checks.|

6. **Examples**

   - Ensembl, Unix OS:
   
   ```
   python MENdelScript.py -o EnsemblExample.csv -c T -p NGG -d -3 -oh 0 -to F -ts 0 -g ens -i ENSDART00000011520 -st 1.5 -t7 F -v F -va F
   ```
   
   - Ensembl, Windows:
   
   ```
   python Windows_MENdelScript.py -o EnsemblCas12aExample.csv -g ens -i ENSDART00000011520 -st 1.5
   ```

   - GenBank, Mac:
   
   ```
   python MENdelScript.py -o GenBankTalenExample.csv -g gb -i AY214391.1
   ```
