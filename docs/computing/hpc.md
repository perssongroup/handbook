## Computing Resources <a name="computing"></a>
Our group’s main computing resources are:

* [NERSC](#nersc) (the LBNL supercomputing center, one of the biggest in the world)

* Lawrencium / Berkeley Research Computing

* Peregrine (the NREL supercomputing center)

* Argonne Leadership Computing Facility (sometimes)

* Oak Ridge Leadership Computing Facility (sometimes)

At any time, if you feel you are computing-limited, please contact Kristin so she can work with you on finding solutions.

### NERSC <a name="nersc"></a>

#### To get started with calculations at NERSC: <a name="startnersc"></a>
1. Ask Kristin about whether you will be running at NERSC and, if so, under what account / repository to charge.
2. Request a NERSC account through the NERSC homepage (Google “NERSC account request”).
3. A NERSC Liason or PI Proxy will validate your account and assign you an initial allocation of computing hours.
4. At this point, you should be able to log in, check CPU-hour balances, etc. through “NERSC NIM” and “My NERSC” portals
5. In order to log in and run jobs on the various machines at NERSC, review the NERSC documentation.
6. In order to load and submit scripts for various codes (VASP, ABINIT, Quantum Espresso), NERSC has lots of information to help. Try Google, e.g. “NERSC VASP”.
... * Note that for commercial codes such as VASP, there is an online form that allows you to enter your VASP license, which NERSC will confirm and then allow you access to.
7. Please make a folder inside your project directory and submit all your jobs there, as your home folder has only about 40GB of space. For example, for matgen project, your work folder path should be something like the following:
`/global/project/projectdirs/matgen/YOUR_NERSC_USERNAME`
8. You can also request a mongo database for your project to be hosted on NERSC. Google “MongoDB on NERSC” for instructions. Donny Winston or Patrick Huck can also help you get set up and provide you with a preconfigured database suited for running Materials Project style workflows.

#### Running Jobs on NERSC
This tutorial provides a brief overview of setting yourself up to run jobs on NERSC. If any information is unclear or missing, feel free to edit this document or contact Kara Fong.

##### Setting up a NERSC account:
Contact the group’s NERSC Liaison (currently Eric Sivonxay, see Group Jobs list). They will help you create an account and allocate you computational resources. You will then receive an email with instructions to fill out the Appropriate Use Policy form, set up your password, etc.

Once your account is set up, you can manage it at the NERSC Information Management (NIM) website.

##### Connecting with SSH:
You must use the SSH protocol to connect to NERSC.
Make sure you have SSH installed on your local computer (you can check this by typing which ssh).
Make sure you have a directory named $HOME/.ssh on your local computer (if not, make it).
Run the command ssh-keygen -t rsa -b 4096. This will generate an RSA key, which you can view in the file id_rsa.pub
You’ll be asked to enter a passphrase, which should be different from your password.

You must store your SSH public key on the NERSC NIM database.
Go to the NIM website, navigate to “My Stuff” -> “My SSH Keys”. Click on the SSH Keys tab.
Copy your key (from id_rsa.pub) into the website’s text box, click Add.

##### Logging on:
Log on to Cori, for example, by submitting the following command in the terminal:
```
ssh username@cori.nersc.gov
```
You will be prompted to enter your passphrase. This will take you to your home directory. You may also find it useful to set up an alias for signing on to HPC resources. To do this, add the following line to your bash_profile:
```
alias cori="ssh your_username@cori.nersc.gov"
```
Now you will be able to initialize a SSH connection to cori just by typing `cori` in the command line and pressing enter.

##### Transferring files to/from NERSC:
For small files, you can use SCP (secure copy). To get a file from NERSC, use:
```
scp user_name@dtn01.nersc.gov:/remote/path/myfile.txt /local/path
```
To send a file to NERSC, use:
```
scp /local/path/myfile.txt user_name@dtn01.nersc.gov:/remote/path
```
To move a larger quantity of data using a friendlier interface, use Globus Online.

##### Running and monitoring jobs:
The following instructions are for running on Cori. Analogous information for running on Edison can be found here.

Most jobs are run in batch mode, in which you prepare a shell script telling the batch system how to run the job (number of nodes, time the job will run, etc.). NERSC’s batch system software is called SLURM. Below is a simple batch script example, copied from the NERSC website:
```
#!bin/bash -l

#SBATCH -N 2          #Use 2 nodes
#SBATCH -t 00:30:00   #Set 30 minute time limit
#SBATCH -q regular    #Submit to the regular QOS
#SBATCH -L scratch    #Job requires $SCRATCH file system
#SBATCH -C haswell    #Use Haswell nodes

srun -n 32 -c 4 ./my_executable
```
Here, the first line specifies which shell to use (in this case bash). The keyword #SBATCH is used to start directive lines ([click here](http://www.nersc.gov/users/computational-systems/cori/running-jobs/batch-jobs/#toc-anchor-3) for a full description of the sbatch options you can specify). The word “srun” starts execution of the code.

To submit your batch script, use `sbatch myscript.sl` in the directory containing the script file.

Below are some useful commands to control and monitor your jobs:
```
sqs -u username 	(Lists jobs for your account)
scancel jobid 	(Cancels a job from the queue)
```

##### Choosing a QOS (quality of service):
You specify which queue to use in your batch file. Use the **debug** queue for small, short test runs, the **regular** queue for production runs, and the **premium** queue for high-priority jobs.

##### Choosing a node type (haswell vs knl):
You may also specify the resource type you would like your job to run on, witnin your batch file. When running on Cori, there are two CPU architectures available, Haswell, and Knights Landing (known as KNL). Running on a Haswell node will afford you high individual core performance with up to 32 cores per node (or 64 threads per node). A KNL node provides a large core count (68 cores per node or 272 threads per node) which is suitable for programs capable of effectively utilizing multithreading. On Cori, there are 2388 Haswell nodes and 9688 KNL nodes.

[TODO: FILL OUT THIS SECTION MORE]

#### Automatic job submission on NERSC: crontab <a name="crontab"></a>
In order to automatically manage job submission at NERSC, you can use crontab. You can submit jobs periodically even when you are not signed in to any NERSC systems and perhaps reduce the queue time from 5-10 days to a few hours. This is possible because of the way jobs are managed in atomate/fireworks. Please make sure you feel comfortable submitting individual jobs via atomate before reading this section.

In atomate, by using --maxloop 3 for example when setting rocket_launch in your my_qadapter.yaml, after 3 trials in each minute if there are still no READY jobs available in your Launchpad Fireworks would stop the running job on NERSC to avoid wasting computing resources. On the other hand, if you have Fireworks available with the READY state and you have been using crontab for a few days, even if the jobs you submitted a few days ago start running on NERSC, they would pull any READY Fireworks and start RUNNING them reducing the turnaround from a few days to a few hours! So how to setup crontab? Please follow the instructions here:
1. ssh to the node where you want to setup the crontab; try one that is easy to remember such as cori01 or edison01; for logging in to a specific node just do for example “ssh cori01” after you log in to the system (Cori in this example).

2. Type and enter: `crontab -e`

3. Now you can setup the following command in the opened vi editor. What it does is basically running the SCRIPT.sh file every 120 minutes of every day of every week of every month of every year (or simply */120 * * * *):
```
*/120 * * * * /bin/bash -l PATH_TO_SCRIPT.sh >> PATH_TO_LOGFILE
```

4. Setup your SCRIPT.sh like the following: (as a suggestion, you can simply put this file and the log file which keeps a log of submission states in your home folder):
```
source activate YOUR_PRODUCTION_CONDA_ENVIRONMENT FW_CONFIG_FILE=PATH_TO_CONFIG_DIR/FW_config.yaml
cd PATH_TO_YOUR_PRODUCTION_FOLDER
qlaunch --fill_mode rapidfire -m 1000 --nlaunches 1
```
The last line of this 3-line file is really what submitting your job inside your production folder with the settings that you set in FW_config.yaml file. See [atomate documentation](https://atomate.org/#documentation-tutorials) for more info.

5. Please make sure to set your PRODUCTION_FOLDER under /global/project/projectdirs/ that has much more space than your home folder and it is also backed up. Make sure to keep an eye on how close you are to disk space and file number limitations by checking https://my.nersc.gov/ periodically.

#### Running Jupyter Notebooks on Cori <a name="jupyter"></a>
Jupyter notebooks are quickly becoming an indispensable tool for doing computational science. In some cases, you might want to (or need to) harness NERSC computing power inside of  a jupyter notebook. To do this, you can use NERSC’s new Jupyterhub system at https://jupyter-dev.nersc.gov/. These notebooks are run on a large memory node of Cori and can also submit jobs to the batch queues (see http://bit.ly/2A0mqrl for details). All of your files and the project directory will be accessible from the Jupyterhub, but your conda envs won’t be available before you do some configuration.

To set up a conda environment so it is accessible from the Jupyterhub, activate the environment and setup an ipython kernel. To do this, run the command “pip install ipykernel”. More info can be found at http://bit.ly/2yoKAzB.

#### Automatic Job Packing with FireWorks <a name="packing"></a>
DISCLAIMER:  Only use job packing if you have trouble with typical job submission. The following tip is not 100% guaranteed to work., and is based on limited, subjective experience on Cori. Talk to Alex Dunn (ardunn@lbl.gov) for help if you have trouble.

The Cori queue system can be unreasonably slow when submitting many (e.g., hundreds, thousands) of small (e.g., single node or 2 nodes) jobs with qos-normal priority on Haswell. In practice, we have found that the Cori job scheduler will give your jobs low throughput if you have many jobs in queue, and you will often only be able to run 5-30 jobs at a time, while the rest wait in queue for far longer than originally expected (e.g., weeks). While there is no easy way to increase your queue submission rate (AFAIK), you can use FireWorks job-packing to “trick” Cori’s SLURM scheduler into running many jobs in serial on many parallel compute nodes with a single queue submission, vastly increasing throughput.

You can use job packing with the “multi” option to rlaunch. This command launches N parallel python processes on the Cori scheduling node, each which runs a job using M compute nodes.

The steps to job packing are:
1. Edit your my_qadapter.yaml file to reserve N * M nodes for each submission. For example, if each of your jobs takes M = 2 nodes, and you want a N = 10 x speedup, reserve 20 nodes per queue submission.
2. Change your rlaunch command to:
```
rlaunch -c /your/config multi N
```

To have each FireWorks process run as many jobs as possible in serial before the walltime, use the --nlaunches 0 option. To prevent FireWorks from submitting jobs with little walltime left (causing jobs to frequently get stuck as “RUNNING”), set the --timeout option. Make sure --timeout is set so that even a long running job submitted at the end of your allocation will not run over your walltime limit. Your my_qadapter.yaml should then have something similar to the following lines:
```
rocket_launch: rlaunch -c /your/config multi 10 --nlaunches 0 --timeout 169200
nodes: 20
```
Typically, setting N <= 10 will give you a good N-times speedup with no problems. There are no guarantees, however, when N > 10-20. Use N > 50 at your own risk!


### Berkeley Research Computing <a name="brc"></a>
Berkeley Research Computing (BRC) hosts the Savio supercomputing cluster. Savio operates on a condo computing model, in which many PI's and researchers purchase nodes to add to the system. Nodes are accessible to all who have access to the system, though priority access will be given to contributors of the specific nodes. BRC provides 3 types of allocations:
*Condo - Priority access for nodes contributed by the condo group.
*Faculty Computing Allowance (FCA) - Limited computing time provided to each Faculty member using Savio.

#### Setting up a BRC account
To get an account on Savio, fill out the form linked below, making sure to select the appropriate allocation. Typically, most students and postdocs will be running on co_lsdi.
http://research-it.berkeley.edu/services/high-performance-computing/getting-account
After your account is made, you'll need to set up 2-factor authentication. We recommend using Google Authenticator, although any OTP manager will work.

[TODO: Fill out this with specific 2-factor setup details]

#### Logging into BRC
To access your shiny new savio account, you'll want to SSH onto the system from a terminal.
```
ssh username@hpc.brc.berekeley.edu
```
You will be prompted to enter your passphrase. This will take you to your home directory. You may also find it useful to set up an alias for signing on to HPC resources. To do this, add the following line to your bash_profile:
```
alias savio="ssh username@hpc.brc.berekeley.edu"
```

#### Running on BRC
Under the condo account co_lsdi, we have exclusive access to 28 KNL nodes. Additionally, we have the ability to run on other nodes at low priority mode.

##### Accessing Software binaries
Software within BRC is managed through modules. You can access precompiled, preinstalled software by loading the desired module.
```
module load <module_name>
```
To view a list of currently installed programs, use the following command:
```
module avail
```
To view the currently loaded modules use the command:
```
module list
```
Software modules can be removed by using either of the following two commands:
```
module unload <module_name>
module purge
```

###### Accessing In-House software packages
The Persson Group maintains their own versions of popular codes such as VASP, GAUSSIAN, QCHEM and LAMMPS. To access these binaries, ensure that you have the proper licenses and permissions, then append the following line to the .bashrc file in your root directory:
```
export MODULEPATH=${MODULEPATH}:/global/home/groups/co_lsdi/sl7/modfiles
```

##### Using Persson Group Owned KNL nodes
To run on the KNL nodes, use the following job script, replacing <executable> with the desired job executable name. To run vasp after loading the proper module, use vasp_std, vasp_gam, or vasp_ncl.

```
#!bin/bash -l
#SBATCH --nodes=1                 #Use 1 node
#SBATCH --ntasks=64               #Use 64 tasks for the job
#SBATCH --qos=lsdi_knl2_normal    #Set job to normal qos
#SBATCH --time=01:00:00           #Set 1 hour time limit for job
#SBATCH --partition=savio2_knl    #Submit to the KNL nodes owned by the Persson Group
#SBATCH --account=co_lsdi         #Charge to co_lsdi accout
#SBATCH --job-name=savio2_job     #Name for the job

mpirun --bind-to core <executable>
```

##### Running on Haswell Nodes(on Low Priority)
To run on Haswell nodes, use the following slurm submission script:
```
#!bin/bash -l
#SBATCH --nodes=1                 #Use 1 node
#SBATCH --ntasks_per_core=1       #Use 1 task per core on the node
#SBATCH --qos=savio_lowprio       #Set job to low priority qos
#SBATCH --time=01:00:00           #Set 1 hour time limit for job
#SBATCH --partition=savio2        #Submit to the haswell nodes
#SBATCH --account=co_lsdi         #Charge to co_lsdi accout
#SBATCH --job-name=savio2_job     #Name for the job

mpirun --bind-to core <executable>
```

### Peregrine <a name="nrel"></a>
NREL's supercomputer Peregrine is reserved for projects and jobs related to silicon-based Li-ion battery research.

#### Getting an account on Peregrine
To request a peregrine account, visit the following link.

https://www.nrel.gov/hpc/user-accounts.html


### Additional resources:
Other Persson group members and the NERSC website are both excellent resources for getting additional help. If that fails, you can reach out to the NERSC Operations staff:

* 1-800-666-3772 (or 1-510-486-8600)

* Computer Operations = menu option 1 (24/7)

* Account Support = menu option 2,  accounts@nersc.gov

* HPC Consulting = menu option 3, or consult@nersc.gov

* Online Help Desk = help.nersc.gov

-------

Authors: Kara Fong, Eric Sivonxay, John Dagdelen

Contact: karafong@lbl.gov
