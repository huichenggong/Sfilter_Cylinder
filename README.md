# Sfilter
This is a tool for basic analysis in Potassium Channel MD simulation.

## 1.Installation
### 1.1 Prepare conda environment
```bash
conda create -n Name_of Env python=3.11 MDAnalysis numpy pandas scipy networkx matplotlib scipy -c conda-forge
conda activate Name_of_Env
```
### 1.2 Install Sfilter using pip
```bash
git clone https://github.com/huichenggong/Sfilter_Cylinder
cd Sfilter_Cylinder
pip install .
```
### 1.3 add ipython kernel
```bash
conda activate Name_of_Env
conda install ipykernel
python -m ipykernel install --user
```

## How to use it ?
```bash
cd test/01-NaK2K/1-Charmm/with_water
count_cylinder.py -pdb ../em.gro \
  -xtc fix_atom_c_100ps.xtc -K POT \
  -SF_seq THR VAL GLY TYR GLY > k_cylinder.out
  
cd ../dry/
count_cylinder.py -pdb em_dry.gro \
  -xtc fix_atom_c_100ps_dry.xtc -K K \
  -SF_seq THR VAL GLY TYR GLY > k_cylinder.out
  # There are two permeation events in this example
  
cd ../../../03-longest_common_sequence/
match_xtck.py \
  -cylinderS 03-state-code/POT_perm_event.out \
  -perm_up 01-xtck/perm_up.dat \
  -perm_down 01-xtck/perm_down.dat
  # match result with xtck
```
The potassium permeation will be saved in `POT_perm_event.out`. Please take a look at the resident time. 
this is the time that the atom stays in the cylinder. This time should be safely smaller than the trajectory 
time step. 

## 2.What can it do?
### 2.1. Count ion permeation  
![permeation](ion-counting.jpg "permeation definition")
Ion permeation is defined by sequentially passing though 4,1,3 compartment.  
We provide a command line tool to run this counting. `count_cylinder.py`    

### 2.2. Track binding site occupancy state
`count_cylinder.py` will print what atom (index) is in each binding site.  

### 2.3. Two output wrapper are provided
`Cylinder_output` You can use this to load the std_out of `count_cylinder.py`. This includes the ion occupancy.  
`Perm_event_output` You can use this to load the POT_perm_event.out. This includes the permeation event.

### 2.4. MSM mechanism analysis
Under development.  
