# panx2-simulation

Details of molecular dynamics simulations in article "Cryo-EM structure of human heptameric pannexin 2 channel" written by Dr. Hang Zhang.

## Dependencies

* **Schrodinger 2018**: https://www.schrodinger.com/
* **Gromacs 2021**：https://www.gromacs.org/
* **charmm-gui version 3.8**：https://www.charmm-gui.org/
* **AmberTools 2020**: https://ambermd.org/AmberTools.php

## Tutorial
### step 0: download the sequence file and coordinate file of protein. 
0_experimental_structure.pdb is the ecperimental coordinate file of panx2 protein;

1_chain_A.pdb is the ecperimental coordinate file of chain A;

1_AA_sequence.fasta is amino acid sequence of chain A.

### step 1: build the missing loop (amino acids 185-187, 267-272, and 374-376) via the prime （a module in schrodinger software).

We build missing loops via the homology modeling function of prime. The input files of this step are  1_chain_A.pdb and 1_AA_sequence.fasta. The output file of this step is 2_A_prime_modelled.pdb. Please make sure that the amino acids solved by wet experiment are not be modelled in this step.

Next, we build the structures of chain B, C, D, E, F Sequentially, and merge them to 3_merge.pdb.

### step 2: prepare the protein structure with schrodinger, including predicting the number of protons on specific atoms and energy-minimizing the whole structure.

Load 3_merge.pdb into schrodinger software and prepare it with Protein preparation wizard. After preparation, the file 4_merge_prepared.pdb was generated.

### step 3: upload the coordinate file to CHARMM-GUI and generate the input files of simulations.

Delete the hydrogen atoms in 4_merge_prepared.pdb and save other atoms in 5_merge_prepared_noH.pdb. After that, submit 5_merge_prepared_noH.pdb to CHARMM-GUI website. The following operations are attached:

* point the protonation states manually with the reference of 4_merge_prepared.pdb;
* select the states of N- and C- terminis manually;
* add disulfide bonds;
* predict the orientation of the protein and the position of lipid bilayers;
* add pore water;
* parameterize the molecules with CHARMM36m force field in this step;
* download the input files of simulation. 

### step4: prepare the topology file of ATP molecule via amber tools:

place four ATP molecules in our coordinate files, and parameterize ATP with AmberTools (antechamber molecule and amb2gro_top_gro.py script).

Up to this step, the preparations of MD simulation were finished. The topology files were saved in "toppar.zip" and the parameter files of each steps were saved in folder "mdp.zip". For repeating our work, please unzip these two files.

### step5: simulation

The coordinate files before and after MD simulation are 6_before_MD.pdb and 6_after_MD.pdb.

The system was also shown here:
![image](https://github.com/shiyu-wangbyte/panx2-simulation/blob/main/media/system_picture.png)

### step6: steered MD

In steered MD, the distance between the mass center of six alpha carbon atom of 34PHE and the mass center of heavy atoms of ATP molecule was defined as co-variable which was restricted from -4 to 3 within 5000000 steps.

The parameter file of steered MD follows:

```
c1: COM ATOMS=567,6250,11933,17616,23299,28982,34665
c2: COM ATOMS=39825-39832

d1: DISTANCE ATOMS=c1,c2 COMPONENTS
d2: DISTANCE ATOMS=c2,805 COMPONENTS

restraint: ...
        MOVINGRESTRAINT
        ARG=d1.z
        AT0=-4  STEP0=0         KAPPA0=0
        AT1=-4  STEP1=200000    KAPPA1=1000
        AT2=3   STEP2=4700000   KAPPA2=1000
        AT3=3   STEP3=5000000   KAPPA3=0
...

PRINT ARG=d1.z,d2.z FILE=plumed_COLVER STRIDE=200
```


The diffusion process is shown here (click the video and download the mp4 file):

[![Watch the video](https://raw.github.com/GabLeRoux/WebMole/master/ressources/WebMole_Youtube_Video.png)](https://github.com/shiyu-wangbyte/panx2-simulation/blob/main/media/ATP_transfer_panx2.mp4)
