# Installation guide for Genome Skim processing pipelines

**Important Note on Conda:** Most of the tools would be installed through the conda distribution within an activated conda environment in the working shell. 

* Whenever you are using any of the pipelines, make sure that you change the env name in the `conda_source.sh` [script](https://github.com/smirarab/skimming_scripts/blob/master/conda_source.sh). Refer to `CONDAENV=GSkim4` in the script where `GSkim4` should be changed to your environment's name corresponding to all the tool installations.
* In the example below, we would change the `conda_source.sh` [script](https://github.com/smirarab/skimming_scripts/blob/master/conda_source.sh) to say `CONDAENV=tutorial`

The `conda_source.sh` [script](https://github.com/smirarab/skimming_scripts/blob/master/conda_source.sh) is meant to allow users to edit the name of their conda environment to easily switch between various configurations. A sample conda environment configuration can be seen [here](https://github.com/smirarab/skimming_scripts/blob/master/Obsolete/environment.yml). 

**Important note on Gurobi** : To be able to run Gurobi and RESPECT, you will need to create an academic license through this [link](https://www.gurobi.com/documentation/9.1/quickstart_mac/obtaining_a_grb_license.html). Downloadn the license and transfer it to whatever computer or cluster you use. This creates a license file and writes it to a default location (press Enter when it asks if you want to store it to the default location). When Gurobi is run, it looks for the license file in the defualt locations. On the expiration of the license, you will have to repeat the procedure after removing the `gurobi.lic` file from its default location.

**Important note on Git** : To be able to clone the repository, you will need to setup your public SSH key on your Github profile. If not done, you can follow the steps [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) under "Generating a new SSH key" and then, following the steps [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) under "Adding a new SSH key to your account".

**Install main tools**

```
mkdir tutorial
cd tutorial

conda create --name tutorial python=3.8
conda activate tutorial

### Order matters here
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --add channels https://conda.anaconda.org/gurobi

### Instal Skmer
conda install skmer==3.2.1
skmer -h

### The following tools should ideally be installed along with skmer. 
### If not, you can always run this command to install them separately.
conda install jellyfish seqtk mash 

### Run this command to install gurobi solver for respect
conda install gurobi 

### Install RESPECT
git clone https://github.com/shahab-sarmashghi/RESPECT.git
cd RESPECT
python setup.py install
cd ..

### BBMap has been made available as a part of the repository and you can use it directly when we clone the repository later
### You can always run the code below to download BBMap separately, if you want to
### wget -O bbmap.tar.gz https://sourceforge.net/projects/bbmap/files/BBMap_39.01.tar.gz/download
### tar xvfz bbmap.tar.gz
### rm bbmap.tar.gz

### FastME has been made available as a part of the repository and you can use it directly when we clone the repository later
### You can also install FastME (to get backbone trees) separately using the following commands
### wget http://www.atgc-montpellier.fr/download/sources/fastme/fastme-2.1.5.tar.gz
### tar xvfz fastme-2.1.5.tar.gz
### chmod +x fastme-2.1.5/binaries/fastme-2.1.5-linux64
### Change "linux64" at the end if using other platforms (linux32 or windows).
### ./fastme-2.1.5/binaries/fastme-2.1.5-linux64 -h

### Cloning the repository 
git clone git@github.com:smirarab/skimming_scripts.git
cd skimming_scripts

### Installing CONSULT-II
git clone https://github.com/bo1929/KRANK.git
make -C KRANK
cd ./KRANK/
wget https://ter-trees.ucsd.edu/data/krank/lib_reps_adpt-k29_w35_h13_b16_s8.tar.gz
tar -zxf ./https://ter-trees.ucsd.edu/data/krank/lib_reps_adpt-k29_w35_h13_b16_s8.tar.gz

###Running the pipeline on sample data
###Remember to edit the conda_source.sh file (~/tutorial/skimming_scripts/conda_source.sh) with your env name; tutorial here
###Keeping all the pipelines in the same directory would be recommended

bash ../fast_skims_pipeline.sh -i ./test/skims/

```

