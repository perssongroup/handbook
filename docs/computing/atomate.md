This guide walks you through the steps for setting up Fireworks and Atomate on the HPC. This guide will generally cover installation on both NERSC and Lawrencium. Installation steps for a local machine or another super computer will differ, but follow similar steps.

# Setup an environment:

Create the conda environment

``` NERSC
module load python/3.6-anaconda-4.4
conda create -n cms python=3.6
```
``` Savio
module load python/3.6
conda create -n cms python=3.6
```
``` Lawrencium
module load python/3.6
conda create -n cms python=3.6
```


Activate the environment and install the base libraries
Note: cms=computational materials science, but feel free to pick your own name

``` bash
source activate cms
conda install numpy scipy matplotlib pandas
```

to install atomate on the environment, use one of the following:

If you are just using atomate and fireworks, with no plans to develop code/ workflows, use the following:
``` bash
pip install atomate
```

For developers:

``` bash
source activate cms
cd ~/.conda/envs/cms
mkdir code
cd code
git clone https://github.com/materialsproject/pymatgen.git
git clone https://github.com/materialsproject/pymatgen-db.git
git clone https://github.com/materialsproject/fireworks.git
git clone https://github.com/materialsproject/custodian.git
git clone https://github.com/hackingmaterials/atomate.git

cd pymatgen
python setup.py develop
pip install -r requirements.txt
cd ..
cd pymatgen-db
python setup.py develop
pip install -r requirements.txt
cd ..
cd fireworks
python setup.py develop
pip install -r requirements.txt
cd ..
cd custodian
python setup.py develop
pip install -r requirements.txt
cd ..
cd atomate
python setup.py develop
pip install -r requirements.txt
cd ..
```

# Configure Fireworks:
## Create Fireworks configuration files
Make directory in environment’s root directory  (/envs/&lt;name&gt;/) called config

```bash
cd ~/.conda/envs/cms
mkdir config
cd config
```

Make 3 files: FW_config.yaml, db.json, and my_launchpad.yaml with the following contents. Replace the teal highlighted text with details specific to your configuration. Note that you can view your [filesystem online](https://my.nersc.gov/filebrowser.php).

### Fireworks configuration (FW_config.yaml)
``` NERSC
CONFIG_FILE_DIR: /global/homes/s/sivonxay/.conda/envs/cms/config
QUEUE_UPDATE_INTERVAL: 5
```
``` Savio
CONFIG_FILE_DIR: /global/home/users/sivonxay/.conda/envs/cms/config
QUEUE_UPDATE_INTERVAL: 5
```
``` Lawrencium
CONFIG_FILE_DIR: /global/home/users/sivonxay/.conda/envs/cms/config
QUEUE_UPDATE_INTERVAL: 5
```
### Database file (db.json)
```
{
    "host": "mongodb03.nersc.gov",
    "port": 27017,
    "database": "fw_db_name",
    "collection": "tasks",
    "admin_password": "<admin_password>",
    "admin_user": "<admin_username>",    
    "readonly_password": "<readonly_password>",
    "readonly_user": "<readonly_username>",
    "aliases_config": null
}
```

### Fireworks LaunchPad file (my_launchpad.yaml)

```
host: mongodb03.nersc.gov
port: 27017
name: 'fw_db_name'
username: '<admin_username>'
password: ‘<admin_password>’
logdir: null
Istrm_lvl: DEBUG
user_indices: []
wf_user_indices: []
```

## Create Fireworker and Queue Launchers:
### Fireworker file (my_fworker.yaml)

``` NERSC
name: nersc_fworker
category: ''
query: '{}'
env:
    db_file: /global/homes/s/sivonxay/.conda/envs/cms/config/db.json
    vasp_cmd: 'srun -N 1 -n 64 -c 4 --cpu_bind=cores vasp_std'
    gamma_vasp_cmd: 'srun -N 1 -n 64 -c 4 --cpu_bind=cores vasp_gam'
    scratch_dir: /global/cscratch1/sd/sivonxay
    incar_update:
```
``` Savio
name: savio_fworker
category: ''
query: '{}'
env:
    db_file: /global/home/users/sivonxay/.conda/envs/cms/config/db.json
    vasp_cmd: 'mpirun --bind-to core vasp_std'
    gamma_vasp_cmd: 'mpirun --bind-to core vasp_gam'
    scratch_dir: /global/scratch/sivonxay/
    incar_update:
```
``` Lawrencium
name: lrc_fworker
category: ''
query: '{}'
env:
    db_file: /global/home/users/sivonxay/.conda/envs/cms/config/db.json
    vasp_cmd: 'mpirun --bind-to core vasp_std'
    gamma_vasp_cmd: 'mpirun --bind-to core vasp_gam'
    scratch_dir: /global/scratch/sivonxay/
    incar_update:
```

### Queue Adapter (my_qadapter.yaml):
``` NERSC
_fw_name: CommonAdapter
_fw_q_type: SLURM
rocket_launch: rlaunch -w /global/homes/s/sivonxay/.conda/envs/cms/config/my_fworker.yaml singleshot
nodes: 1
walltime: '24:00:00'
account: matgen
job_name: knl_launcher
qos: regular
constraint: 'knl'
pre_rocket: |
            source activate cms
            module load vasp-tpc/5.4.4-knl
            export OMP_PROC_BIND=true
            export OMP_PLACES=threads
            export OMP_NUM_THREADS=1
```
``` Savio
_fw_name: CommonAdapter
_fw_q_type: SLURM
rocket_launch: rlaunch -w /global/home/users/sivonxay/.conda/envs/cms/config/my_fworker.yaml singleshot
nodes: 1
walltime: '24:00:00'
account: co_lsdi
job_name: knl_launcher
queue: savio2_knl
qos: lsdi_knl2_normal
ntasks: 64
pre_rocket: |
            source activate cms
            module load vasp
            export OMP_PROC_BIND=true
            export OMP_PLACES=threads
            export OMP_NUM_THREADS=1
            post_rocket: null
```
``` Lawrencium
```

For more information on best practices for running VASP on multiple nodes (i.e. how to set vasp_cmd in my_fworker.yaml based on the number of nodes requested in my_qadapter) see the [NERSC vasp training](https://www.nersc.gov/users/training/events/vasp-user-hands-on-knl-training-june-18-2019/)

## Configure Bash profile:

Append the following lines to the .bashrc.ext file (which is located in your home directory, e.g. /global/homes/s/sivonxay)

```NERSC
module load python/3.6-anaconda-4.4
export PMG_VASP_PSP_DIR=/project/projectdirs/matgen/POTCARs
export FW_CONFIG_FILE='/global/homes/s/sivonxay/.conda/envs/cms/config/FW_config.yaml'
alias cdconfig='cd ~/.conda/envs/cms/config'
```
```Savio
module load python/3.6
export PMG_VASP_PSP_DIR=/clusterfs/mp/software/POTCARs
export FW_CONFIG_FILE='/global/home/users/sivonxay/.conda/envs/cms/config/FW_config.yaml'
alias cdconfig='cd ~/.conda/envs/cms/config'
```
```Lawrencium
```

Note the line alias cdconfig='cd ~/.conda/envs/cms/config' is optional but is recommended so you can more easily locate the directory with your configuration files stored by typing cdconfig into the command line.

## Setup API key and Add POTCAR Directory to pymatgen
Go to materialsproject.org and get an API key. Make sure you are using the environment setup above to run these commands.

```bash
pmg config --add PMG_MAPI_KEY <USER_API_KEY>
pmg config --add PMG_VASP_PSP_DIR /project/projectdirs/matgen/POTCARs
```

Running these commands will create a .pmgrc.yaml file (if it doesn’t exist already) containing these configuration settings in your home directory

# Using Fireworks and Atomate:
## Initializing Fireworks Database:
Use care when running this command. It will wipe all existing entries in your fireworks database in the fireworks, workflows, and launches collections.

``` bash
lpad reset
```

## Using Atomate to create preset workflow
The following code blocks are python code that should be run in jupyter or from the terminal. Make sure you are using the environment setup above. The computer running the code should have access to mongodb03.nersc.gov; this can be disregarded when running directly on nersc or when connected to to the LBNL intranet. For computers outside of LBNL, a VPN will need to be used.

Run the following python code by creating a file named “make_workflow.py”:
```python
from pymatgen import MPRester
from fireworks import LaunchPad
from atomate.vasp.workflows.presets.core import wf_static
from atomate.vasp import powerups

mpr = MPRester()

structure = mpr.get_structure_by_material_id(“mp-145”)
wf = wf_static(structure)
wf = powerups.add_common_powerups(wf, {"scratch_dir":">>scratch_dir<<"})

lp = LaunchPad.auto_load()
lp.add_wf(wf)
```
The previous python script added a static energy calculation for Hexagonal Lu to your job database.

```bash
python “make_workflow.py
```

To check on the job, run the command:

```bash
lpad get_fws -s READY
```

## Running Jobs:
We recommend using a tmux or screen when submitting jobs to preserve queue submission when the main ssh session is terminated. This allows one to keep the queue saturated with jobs. For example, for tmux:

```bash
module load tmux
tmux new -s background_launcher
source active cms
mkdir FireworksTest
cd FireworksTest
```

To exit the tmux session, press the “control” and “b” keys together, followed by “d”

To re-enter the tmux session:

```bash
tmux attach -t background_launcher
```

For more tmux commands, see the [tmux cheatsheet](https://tmuxcheatsheet.com/)

Use qlaunch command to reserve jobs with SLURM’s scheduler. Qlaunch has 3 modes; singleshot, rapidfire, and multi. Singleshot is used to launch one job, rapidfire is used to launch multiple jobs in quick succession, and multi creates one job with multiple fireworks runs. You’ll probably want to use rapidfire. Some useful flags to set are: -m to specify maximum # of jobs in queue at any given time and --nlaunches to specify how many fireworks to run.
Here are some examples:
``` example_1
qlaunch singleshot
```
``` example_2
qlaunch rapidfire --nlaunches 10
```
``` example_3
qlaunch rapidfire --nlaunches infinite -m 50
```
``` example_4
qlaunch -fm rapidfire --nlaunches infinite -m 50
```

## Monitoring Database/ Fireworks
Status of running fireworks can be determined by using the command
``` example_1
lpad get_fws -s RUNNING
```
``` example_2
lpad get_fws -q '{"state":"RUNNING", "fw_id":{"$gte": 50}}'
```

Rerunning fizzled (Failed) fireworks
``` example_1
lpad rerun_fws -i <fw_id>
```
``` example_2
lpad rerun_fws -s FIZZLED
```

Alternatively, you can view your jobs in the GUI by downloading your my_launchpad.yaml file to your local machine.
To do so:

```bash
scp <username>@dtn01.nersc.gov:~/.conda/envs/cms/config/my_launchpad.yaml .
```

Then, run this command, locally, in the folder containing the my_launchpad.yaml file. This will bring up the web interface hosted on you local machine on port 5000 (127.0.0.1:5000).

```bash
lpad webgui
```

Occasionally, jobs may hang and not complete. To check for hung jobs, use the command:

```bash
lpad detect_lostruns
```

To rerun these jobs, add the --rerun flag to the command.

```bash
lpad detect_lostruns --rerun
```

# Viewing Database and Outputs:
Download and install Robo 3T from the following [link](https://robomongo.org/download)
## Setup connection using mongodb credentials
![](/resources/mongodb_1.png)
![](/resources/mongodb_2.png)

## Querying collections

-----

Authors: Eric Sivonxay, Julian Self, Ann Rutt, and Oxana Andriuc

Contact: esivonxay@lbl.gov and jself@lbl.gov
