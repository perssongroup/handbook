## Computing Resources <a name="computing"></a>
Our group’s main computing resources are:

* [NERSC](#nersc) (the LBNL supercomputing center, one of the biggest in the world)
* [Lawrencium](http://scs.lbl.gov) / Berkeley Lab Research Computing
* [Savio](https://research-it.berkeley.edu/services/high-performance-computing/system-overview) / Berkeley Research Computing
* [Peregrine](https://www.nrel.gov/hpc/about-hpc.html) (the NREL supercomputing center)
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

##### Logging on (Setup):

You must use the SSH protocol to connect to NERSC.
Make sure you have SSH installed on your local computer (you can check this by typing `which ssh`).

##### Connecting with SSH:
You must use the SSH protocol to connect to NERSC.
Make sure you have SSH installed on your local computer (you can check this by typing which ssh) and that you have a directory named $HOME/.ssh on your local computer (if not, make it).

You will also need to [set up multi-factor authentication with NERSC](https://www.nersc.gov/users/connecting-to-nersc/mfa/). This will allow you to generate "one time passwords" (OTPs). You will need append a OTP to the end of your NIM password each time you log on to a NERSC cluster.

We also advise you to configure a ssh socket so that you only have to log into NERSC with a OTP only once per session (helpful if you are scp-ing things). To do this:

1. Create the directory `~/.ssh/sockets` if it doesn't already exist.
2. Open your ssh config file `/.ssh/config` (or create one if it doesn't exist) and add the following:
```
Host *.nersc.gov
ControlMaster auto
ControlPath ~/.ssh/sockets/%r@%h-%p
ControlPersist 600
```

You should now be ready to log on!

---

You must store your SSH public key on the NERSC NIM database.
Go to the NIM website, navigate to “My Stuff” -> “My SSH Keys”. Click on the SSH Keys tab.
Copy your key (from id_rsa.pub) into the website’s text box, click Add.

##### Logging on:

Log on to Cori, for example, by submitting the following command in the terminal:
```
ssh username@cori.nersc.gov
```
You will be prompted to enter your passphrase, which is your NIM password+OTP (e.g. `<your_password><OTP>` without any spaces). This will log you onto the cluster and take you to your home directory. You may also find it useful to set up an alias for signing on to HPC resources. To do this, add the following line to your bash_profile:
```
alias cori="ssh <your_username>@cori.nersc.gov"
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
To move a larger quantity of data using a friendlier interface, use [Globus Online](http://globus.lbl.gov).

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
sqs -u username (Lists jobs for your account)
scancel job_id 	(Cancels a job from the queue)
```

##### Choosing a QOS (quality of service):
You specify which queue to use in your batch file. Use the **debug** queue for small, short test runs, the **regular** queue for production runs, and the **premium** queue for high-priority jobs.

##### Choosing a node type (haswell vs knl):
You may also specify the resource type you would like your job to run on, witnin your batch file. When running on Cori, there are two CPU architectures available, Haswell, and Knights Landing (known as KNL). Running on a Haswell node will afford you high individual core performance with up to 32 cores per node (or 64 threads per node). A KNL node provides a large core count (68 cores per node or 272 threads per node) which is suitable for programs capable of effectively utilizing multithreading. On Cori, there are 2388 Haswell nodes and 9688 KNL nodes.

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
_Please make sure you will actually be performing work on Savio before requesting an account._ To get an account on Savio, fill out the [form](https://docs.google.com/forms/d/e/1FAIpQLScOiGy3v3ZvxqHAKPiU-GNwA_WmUoUUgXKXPZ1j_5dMOS2m6Q/viewform) linked below, making sure to select the appropriate allocation. Typically, most students and postdocs will be running on co_lsdi. For more information, visit (Berkeley Research Computing)[http://research-it.berkeley.edu/services/high-performance-computing]

After your account is made, you'll need to [set up 2-factor authentication](https://research-it.berkeley.edu/services/high-performance-computing/logging-brc-clusters). This will allow you to generate "one time passwords" (OTPs). You will need append a OTP to the end of your NIM password each time you log on to a NERSC cluster. We recommend using Google Authenticator, although any OTP manager will work.

#### Logging on (Setup):

You must use the SSH protocol to connect to BRC.
Make sure you have SSH installed on your local computer (you can check this by typing `which ssh`).
Make sure you have a directory named $HOME/.ssh on your local computer (if not, make it).

We also advise you to configure a ssh socket so that you only have to log into BRC with a OTP only once per session (helpful if you are scp-ing things). To do this:

1. Create the directory `~/.ssh/sockets` if it doesn't already exist.
2. Open your ssh config file `/.ssh/config` (or create one if it doesn't exist) and add the following:
```
Host *.brc.berekeley.edu
ControlMaster auto
ControlPath ~/.ssh/sockets/%r@%h-%p
ControlPersist 600
```

After your account is made, you'll need to set up 2-factor authentication. We recommend using Google Authenticator, although any OTP manager will work.

You should now be ready to log on!

#### Logging on to BRC
To access your shiny new savio account, you'll want to SSH onto the system from a terminal.
```
ssh your_username@hpc.brc.berekeley.edu
```
You will be prompted to enter your passphrase+OTP (e.g. `<your_password><OTP>` without any spaces). This will take you to your home directory. You may also find it useful to set up an alias for signing on to HPC resources. To do this, add the following line to your bash_profile:
```
alias savio="ssh your_username@hpc.brc.berekeley.edu"
```
Now you will be able to initialize a SSH connection to Savio just by typing `savio` in the command line and pressing enter.

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



### LBNL Laboratory Research Computing <a name="lrc"></a>
Lawrence Berkeley National Laboratory's *Laboratory Research Computing* (LRC) hosts the Lawrencium supercomputing cluster. LRC operates on a condo computing model, in which many PI's and researchers purchase nodes to add to the system. Nodes are accessible to all who have access to the system, though priority access will be given to contributors of the specific nodes. BRC provides 3 types of allocations:
*Condo - Priority access for nodes contributed by the condo group.
*PI Computing Allowance (PCA) - Limited computing time provided to each PI member using Lawrencium.

#### Persson Group ES1 GPU Node Specs:
<table style="margin-bottom:1em;border-collapse:collapse;border-spacing:0px;padding:0px;color:rgb(51,51,51);font-family:helvetica neue,helvetica,arial,sans-serif;font-size:14px;line-height:19.6px;background-color:rgb(245,245,245)">
<tbody style="border-top:1px solid rgb(213,213,213)">
<tr>
<th colspan="2" style="border-bottom:0px none rgb(204,204,204);border-right-color:rgb(204,204,204);border-left-color:rgb(204,204,204);padding:5px;width:760px;height:19px;background-color:rgb(238,238,238)">GPU Computing Node</th>
</tr>
<tr>
<td style="padding:5px;border-color:rgb(204,204,204);width:103px;height:19px">Processors</td>
<td style="padding:5px;border-color:rgb(204,204,204);width:657px;height:19px">Dual-socket,&nbsp;<span style="line-height:19.6px">4-core, 2.6GHz Intel 4112 processors</span>&nbsp;(8 cores/node)</td>
</tr>
<tr>
<td style="padding:5px;border-color:rgb(204,204,204);width:103px;height:19px">Memory</td>
<td style="padding:5px;border-color:rgb(204,204,204);width:657px;height:19px">192GB (8 X 8GB) 2400Mhz DDR4 RDIMMs</td>
</tr>
<tr>
<td style="padding:5px;border-color:rgb(204,204,204);width:103px;height:19px">Interconnect</td>
<td style="padding:5px;border-color:rgb(204,204,204);width:657px;height:19px">56Gb/s Mellanox ConnectX5 EDR Infiniband interconnect</td>
</tr>
<tr>
<td style="padding:5px;border-color:rgb(204,204,204);width:103px;height:19px">GPU</td>
<td style="padding:5px;border-color:rgb(204,204,204);width:657px;height:19px">2 ea. Nvidia Tesla v100 accelerator boards</td>
</tr>
<tr>
<td style="padding:5px;border-color:rgb(204,204,204);width:103px;height:19px">Hard Drive</td>
<td style="padding:5px;border-color:rgb(204,204,204);width:657px;height:19px">500GB SSD (Local swap and log files)</td>
<br>
</td>
</tr>
</tbody>
</table>

#### Setting up a LRC account
_Please make sure you will actually be performing work on Lawrencium before requesting an account._ To get an account on Lawrencium, fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLScTqDeiv8glP88YhAyePDE4ZDZcv_yhFQVUn4roVwZi984iyw/viewform) and the [user agreement](http://sites.google.com/a/lbl.gov/high-performance-computing-services-group/useragreement) to a one-time password token generator and your account. You will also need to [set up a MFA token for your account](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/authentication/linotp-usage).

#### Before logging on (setup)

You must use the SSH protocol to connect to Lawrencium.
Make sure you have SSH installed on your local computer (you can check this by typing `which ssh`).
Make sure you have a directory named $HOME/.ssh on your local computer (if not, make it).

After your account is made, you'll need to set up 2-factor authentication. We recommend using Google Authenticator, although any OTP manager will work.

You should now be ready to log on!

#### Logging on to LRC
To access your shiny new Lawrencium account, you'll want to SSH onto the system from a terminal.
```
ssh your_username@lrc-login.lbl.gov
```
You will be prompted to enter your pin+OTP (e.g. `<your_pin><OTP>` without any spaces). This will take you to your home directory. You may also find it useful to set up an alias for signing on to HPC resources. To do this, add the following line to your bash_profile:
```
alias lawrencium="ssh <your_username>@lrc-login.lbl.gov"
```
Now you will be able to initialize a SSH connection to Savio just by typing `lawrencium` in the command line and pressing enter.

#### Running on LRC
Under the condo accounts condo_mp_cf1 (56 cf1 nodes) and condo_mp_es1 (1 gpu node), we have exclusive access to certain Lawrencium nodes. If you do not know which of these node groups you are supposed to be running on, you probably shouldn't be running on Lawrencium. Additionally, we have the ability to run on ES1 GPU nodes at low priority mode (es_lowprio).

##### Accessing Software binaries
Software within LRC is managed through modules. You can access precompiled, preinstalled software by loading the desired module.
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

#### Using Persson Group Owned LRC nodes
To run on the  nodes, use the following job script, replacing <executable> with the desired job executable name.

```CF1_KNL
#!/bin/bash
# Job name:
#SBATCH --job-name=<job_name>
#
# Partition:
#SBATCH --partition=cf1
#
# QoS:
#SBATCH --qos=condo_mp_cf1
#
# Account:
#SBATCH --account=lr_mp
#
# Nodes (IF YOU CHANGE THIS YOU MUST CHANGE ntasks too!!!):
#SBATCH --nodes=1
#
# Processors (MUST BE 64xNUM_NODES ALWAYS!!!):
#SBATCH --ntasks=64
#
# Wall clock limit:
#SBATCH --time=24:00:00

## Run command

module load vasp/6.prerelease-vdw
export OMP_PROC_BIND=true
export OMP_PLACES=threads
export OMP_NUM_THREADS=1 # NEVER CHANGE THIS!!

mpirun --bind-to core <executable>
```
```ES1_condo
#!/bin/bash
# Job name:
#SBATCH --job-name=<job_name>
#
# Partition:
#SBATCH --partition=es1
#
# QoS:
#SBATCH --qos=condo_mp_es1
#
# Account:
#SBATCH --account=lr_mp
#
# GPUs:
#SBATCH --gres=gpu:2
#
# CPU cores:
#SBATCH --cpus-per-task=8
#
# Constraints:
#SBATCH --constraint=es1_v100
#
# Wall clock limit:
#SBATCH --time=24:00:00

export CUDA_VISIBLE_DEVICES=0,1
module load cuda/10.0
```
```ES1_lowprio
#!/bin/bash
# Job name:
#SBATCH --job-name=<job_name>
#
# Partition:
#SBATCH --partition=es1
#
# QoS:
#SBATCH --qos=es_lowprio
#
# Account:
#SBATCH --account=lr_mp
#
# GPUs:
#SBATCH --gres=gpu:2
#
# CPU cores:
#SBATCH --cpus-per-task=8
#
# Constraints:
#SBATCH --constraint=es1_v100
#
# Wall clock limit:
#SBATCH --time=24:00:00

export CUDA_VISIBLE_DEVICES=0,1
module load cuda/10.0
```

#### Interactive Jobs on the Group GPU Condo Node
To run an interactive session on the GPU node, use the following two commands to provision and log in to the node: 
`salloc --time=24:00:00 --nodes=1 -p es1 --gres=gpu:2 --cpus-per-task=8 --qos=condo_mp_es1 --account=lr_mp`
`srun --pty -u bash -i`

### Additional resources
Other Persson group members and the NERSC website are both excellent resources for getting additional help. If that fails, you can reach out to the NERSC Operations staff:

* 1-800-666-3772 (or 1-510-486-8600)

* Computer Operations = menu option 1 (24/7)

* Account Support = menu option 2,  accounts@nersc.gov

* HPC Consulting = menu option 3, or consult@nersc.gov


---

Authors: Kara Fong, Eric Sivonxay, John Dagdelen
Contact: karafong@lbl.gov
