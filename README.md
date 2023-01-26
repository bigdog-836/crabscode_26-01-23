# crabscode_26-01-23
## if I see (base), type: conda deactivate (May need to do this twice)
### Installation - One and done

1) git clone https://github.com/gjeunen/reference_database_creator.git
6) conda create -p /nesi/nobackup/uoo0304/ryan/bin/crabs_packages
7) conda activate /nesi/nobackup/uoo03004/ryan/bin/crabs_packages
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
5) module load Miniconda3/4.12.0
15) conda activate /nesi/nobackup/uoo03004/ryan/bin/crabs_packages
17) export PATH="/nesi/nobackup/uoo03004/ryan/bin/reference_database_creator:$PATH"
18) 
