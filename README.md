# crabscode_26-01-23
## Following https://github.com/gjeunen/reference_database_creator#manual-installation
## if I see (base), type: conda deactivate (May need to do this twice)
### Installation - One and done
1a) module load Miniconda3/22.11.1-1
1) git clone https://github.com/gjeunen/reference_database_creator.git #do this in bin
6) conda create -p /nesi/project/uoo03904/bin/crabs_packages
7) conda activate /nesi/project/uoo03904/bin/crabs_packages
8) conda install argparse
9) conda install biopython
10) conda install tqdm
11) conda install numpy
12) conda install matplotlib
13) conda install pandas

### Installation - Need to be run everytime

2) module load cutadapt/4.1-gimkl-2022a-Python-3.10.5
3) module load VSEARCH/2.21.1-GCC-11.3.0
4) module load MUSCLE/3.8.1551
5) module load Miniconda3/22.11.1-1
15) conda activate /nesi/project/uoo03904/bin/crabs_packages
17) export PATH="/nesi/project/uoo03904/bin/reference_database_creator:$PATH"
18) crabs -h (to test installation)

### Now start Mitofish

19) crabs db_download --source mitofish --output mitofish.fasta --keep_original yes

### Now start NCBI
#12S

20) crabs db_download --source ncbi --database nucleotide --query '12S[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output 12S_NCBI_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000

#COI 

21) crabs db_download --source ncbi --database nucleotide --query 'COI[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output COI_NCBI_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000

#CytB

22) crabs db_download --source ncbi --database nucleotide --query 'CytB[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output CytB_NCBI_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000

#16S

23) crabs db_download --source ncbi --database nucleotide --query '16S[All Fields] AND txid7898[Organism:exp] AND mitochondrion[filter]' --output 16S_NCBI_fish.fasta --keep_original yes --email ryan.r.easton@gmail.com --batchsize 5000

### Now start EMBL (sbatch submission = sbatch filename. To monitor progress squeue -u easry078)

21) crabs db_download --source embl --database 'VRT*' --output embl_vrt.fasta --keep_original yes

## Now start BOLD 

22)  crabs db_download --source bold --database 'Actinopterygii|Petromyzontiformes' --output bold_fish_lamprey.fasta --keep_original yes

## Assign taxonomy

24) crabs db_download --source taxonomy

## db_merge

25) crabs db_merge --output merged_total.fasta --uniq no --input mitofish.fasta 12S_NCBI_fish.fasta bold_fish_lamprey.fasta COI_NCBI_fish.fasta CytB_NCBI_fish.fasta 16S_NCBI_fish.fasta embl_vrt.fasta

## insilico_pcr (Table 1 https://onlinelibrary-wiley-com.ezproxy.otago.ac.nz/doi/pdf/10.1002/ece3.7658)

26) crabs insilico_pcr --input merged_total.fasta --output pcr_16s_Ac16s.fasta --fwd CCTTTTGCATCATGATTTAGC --rev CAGGTGGCTGCTTTTAGGC --error 4.5

## pga

27) crabs pga --input ../merged_total.fasta --database pcr_Cytb_Tania.fasta --output pga_Cytb_Tania.fasta --fwd GAAAAACCACCGTTGTTATTCA --rev CGACTTCCGGATTACAAGACC --speed medium --percid 0.95 --coverage 0.95 --filter_method relaxed

## assign_tax

28) crabs assign_tax --input pga_Cytb_Tania.fasta --output Cytb_Tania.tsv --acc2tax ../nucl_gb.accession2taxid --taxid ../nodes.dmp --name ../names.dmp --missing missing_taxa_Cytb_Tania.tsv

## dereplicate

29) crabs dereplicate --input Cytb_Tania.tsv --output Derep_Cytb_Tania.tsv --method uniq_species

## seq_cleanup

30) crabs seq_cleanup --input input.tsv --output output.tsv --minlen 100 --maxlen 500 --maxns 0 --enviro yes --species yes --nans 0
