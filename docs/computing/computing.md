<a name="personal_setup"></a>
## Setting up a new Macbook

### Upgrade your OS
If your computer is not using the latest OS, you should upgrade to the latest OS first.

### Installing Python development environment
The best way to manage Python installations these days is a “conda env”. This will allow you to manage different Python “environments”, where each environment is a set of libraries that you have installed. For example, you can have one environment that uses Python 2.7 and has certain library versions installed, and another environment that uses Python 3.5 and has other libraries installed. Another advantage of conda environments is that you can apply the same procedure on NERSC and other computing centers that support conda.

To do this, follow the online instructions on installing a conda environment and see modifications below:
* http://conda.pydata.org/docs/using/index.html
* (probably) prefer to install the “miniconda” version rather than anaconda
* (probably) prefer to install “miniconda 3” rather than “miniconda 2”. Both will work fine and allow you to do everything the other one does so don’t stress too much about this decision.
* When creating environments, use a command like this (note that this also installs recommended libraries):
```
conda create --name py3 python=3 numpy matplotlib seaborn plotly pandas flask pymongo scipy sympy scikit-learn jupyter
```
* If you want a reference guide to conda commands, try: http://conda.pydata.org/docs/using/cheatsheet.html

### Install high-throughput computation environment
Our group has a set of base codebases used for performing high-throughput calculations. Note that if your project does not involve high-throughput calculation, you may need only one or two of these libraries installed – ask your subgroup head if you are unsure.

**After activating a conda environment**, install the following packages using a combination of git clone >>REPO_NAME<< and python setup.py develop. Start with:
```
git clone https://www.github.com/materialsproject/fireworks
```
You might need to generate an ssh key for the git clone command to work:
ssh-keygen -t rsa -b 4096
no password is probably OK unless you are security conscious
add your SSH key to your Github profile

Then:
```
cd fireworks; python setup.py develop; cd ..
```
Repeat the process above for the remaining libraries:
```
git clone https://www.github.com/materialsproject/pymatgen
cd pymatgen; python setup.py develop; cd ..
git clone https://www.github.com/materialsproject/pymatgen-db
cd pymatgen-db; python setup.py develop; cd ..
git clone https://www.github.com/materialsproject/custodian
cd custodian; python setup.py develop; cd ..
```
Repeat the same process for a couple of other libraries on the hackingmaterials github site:
```
git clone https://www.github.com/hackingmaterials/atomate
cd atomate; python setup.py develop; cd ..
git clone https://www.github.com/hackingmaterials/matminer
cd matminer; python setup.py develop; cd ..
```
If you want, you can automatically source activate your environment in your .bash_profile file. This will automatically load your environment when you open a Terminal. Otherwise, you will start off in your default Mac Python.

### Other things to do
* Set up your Time Machine backup (make sure you have purchased or received an external hard disk).
https://support.apple.com/en-us/HT204412
* You can also set up an online backup plan (e.g., Crashplan or Backblaze) to provide you with a second backup.
* Install MongoDB.
* Install Docker.
* Purchase Microsoft office from LBNL software distribution.

-----------------------------------------------------------------------------------------------------------------------------

## Printing and Scanning <a name="printing"></a>
The group has a Canon MF731C printer located in 33-143C. It supports color printing as well as two-sided printing and includes a scanner. To connect to the printer follow [this guide on the internal group site](https://sites.google.com/a/lbl.gov/perssongroup/printers-in-persson-group).

-----------------------------------------------------------------------------------------------------------------------------

## Software help groups <a name="softwaresupport"></a>
If you have problems with software, and in particular the software maintained by our group and our collaborators, you should contact the appropriate help group. The documentation for the software will list what that channel is; if not, try the Github Issues page. If you are reaching out for help, try to provide everything needed to quickly reproduce and debug the problem (files, test code, etc).

Two other ways to get software help that are more self-guided are:

* If you are having trouble using a particular class or function, look for unit tests within the code, which often demonstrate how to use the class or function

* If the class or function has a unique name (e.g., MaterialsProjectCompatibility), another option is to both Google and search on github.com for the particular class/function. The github.com search will often reveal code snippets from users all around the world.

---
Authors: Kara Fong

Contact: karafong@lbl.gov
