# panx2-simulation

Details of molecular dynamics simulations in article "Cryo-EM structure of human heptameric pannexin 2 channel" written by Dr. Hang Zhang.

## Dependencies

* **Schrodinger 2018**: https://www.schrodinger.com/
* **Gromacs 2021**：https://www.gromacs.org/
* **charmm-gui version 3.8**：https://www.charmm-gui.org/
* **CGenFF Tool**: https://cgenff.umaryland.edu/

## Tutorial
### step 0: download the sequence file and coordinate file of protein. 

### step 1: build the missing loop (amino acids 185-187, 267-272, and 374-376) via the prime （a module in schrodinger software).
### step 2: prepare the protein structure with schrodinger, including predicting the number of protons on specific atoms and energy-minimizing the whole structure.
### step 3: upload the coordinate file to CHARMM-GUI and generate the input files of simulations.
