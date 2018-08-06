## Getting a Computer

Most long-term appointments (graduate student, postdoc, staff) will mean purchasing a new computer. Short-term appointments (e.g., internships) will not involve a computer purchase unless otherwise stated - you will instead receive an excellent computer from the group’s stock.

### Mac, Windows, or Linux?
You should buy a Mac, and probably a Macbook Pro. Although this sounds extreme, and may even induce strong feelings if you are used to a different system, in practice this has never been much of a problem. We are not necessarily Apple fanatics but simply find that these are the best systems for our type of work because they contain many of the advantages of both Linux and Windows systems in a single package.

Why not Windows? A couple of things make it non-optimal for our work. There is no native Terminal, which you will use heavily, and programs like Cygwin are poor substitutes. Certain seemingly minor decisions made by Windows (directory slashes, line endings) are different than those from Linux, making interoperability between Linux/Mac and Windows systems more problematic (e.g., copying files to and from supercomputing centers requires auto-converting line ending format).

Why not Linux? Linux is fine, but Microsoft Office is not available (which is used by us and most of the materials science research world) and OpenOffice is a poor substitute. Certain videoconferencing software doesn’t work well with Linux.

How about Mac? As Apple are catering more to the general consumer and less to developers, you really need to spend some time setting up your Mac to make it productive for power users. But for the moment it remains a very good compromise between Linux-like and Windows-like and thus forms the basis for our workstations. 

### Purchasing
Here is how to purchase a computer at the lab. Before we begin, a few notes:
In terms of the mechanics of purchasing:
* Use LBNL Ebuy (not Ebay) wherever possible - you need to be on the lab network (onsite via an ethernet cable) or be connected via the VPN
* Use Amazon, etc. to buy various components if not available via EBuy
* The laptop is government property; you are expected to return it to the group when you are done working at LBNL. Note that Mac computers make it very simple to transfer everything over to your next computer.
* You are free to take your laptop home, on trips, etc., unless you are an intern in which case other restrictions may apply from the internship program.
* The lab receives your computer and tags it before sending it over to you.
* You must back up your computer very regularly (at least once per week, ideally continuously). This is simple using the Time Machine app. Just plug your backup drive into your monitor so when you connect to your monitor, you also back up. If there are (for some reason) errors in backing up, fix that issue immediately. There are zero excuses for not doing this.

### Selecting a computer, monitor, and accessories
Your computer workstation is one area where you should just get whatever you think will make you most productive and not care about cost. Seriously, just get what is best and do not worry about cost.

For the computer, you should select a Macbook Pro (any screen size) as mentioned above. You can use the Apple website to browse details. Many of us use a 13” Macbook Pro. It is powerful enough to do serious work and light/small enough to use on a plane. A 15” Macbook Pro is also a good choice. If you would like to get anything other than a Macbook Pro, talk to some senior members of the group first to get their input.

For the monitor, a number of us use a single Thunderbolt display. While this is no longer available for purchase, there may be one or two floating around the group that you can adopt. Another good option available is the LG 27MU88-W (4K resolution) monitor which is on Ebuy. Note that one big screen is usually better ergonomically than dual monitors, and you can use the “Spaces” feature of Mac OS/X to quickly flip between virtual screens if needed. 

For accessories, make sure to get:
* An extra charging cable
* A VGA adapter dongle
* An ethernet cable adapter dongle
* A Time Machine hard disk (for backup). 4TB is a good size. 
* A keyboard. Many of us use an Apple Wireless Keyboard since we like the feel of Mac keys and also like a consistent feel between our laptop keyboard and our desk keyboard. Others prefer mechanical keyboards and some prefer to use the loudest keyboard that they can find (Shyam). If you prefer a larger or ergonomic keyboard, you can certainly get that. 
* A mouse/trackpad. We suggest Apple Magic Trackpad because Mac OS has really customized a lot of the interface for the trackpad (e.g., gestures). Some also value consistency between their laptop and desk workstation. After a while you get used to doing everything on your trackpad even if you were previously very productive/accurate with a mouse on Windows. However, many of us get by just fine with a mouse (especially the Apple Magic Mouse, which has some gesture support.)
* (optional) A presentation tool, e.g., Logitech R800.

### Making the purchase
1. Provide all the details of your selections in an email and send to Alice Mueller. If all looks OK, she will give you a project and activity ID. 
2. Go to eBuy, and for items available there, add them to your cart and submit the requisition with the project and activity ID, and SAS approver as Micah Liedeker [TODO: AGAIN, CHECK THIS].
3. For items not available on eBuy, contact esdradmin@lbl.gov (and cc Kristin and Alice) to obtain a procurement form. Fill it out with item details (Vendor, website, price, etc.) and send it back. 
4. If you select the overnight shipping option (ask Kristin about this and the related extra costs) most parts, except the computer, will arrive within a week to 10 days. The computer needs to be tagged by the lab, so with overnight shipping, it should arrive within 2 weeks. Ideally, you will select your computer well before arriving at the lab and won’t need overnight shipping.

-----------------------------------------------------------------------------------------------------------------------------

## Setting up a new Macbook

### Upgrade your OS
If your computer is not using the latest OS, you should upgrade to the latest OS first.

### Installing Python development environment
The best way to manage Python installations these days is a “conda env”. This will allow you to manage different Python “environments”, where each environment is a set of libraries that you have installed. For example, you can have one environment that uses Python 2.7 and has certain library versions installed, and another environment that uses Python 3.5 and has other libraries installed. Another advantage of conda environments is that you can apply the same procedure on NERSC and other computing centers that support conda.

To do this, follow the online instructions on installing a conda environment and see modifications below:
* http://conda.pydata.org/docs/using/index.html
* (probably) prefer to install the “miniconda” version rather than anaconda
* (probably) prefer to install “miniconda 3” rather than “miniconda 2”. Both will work fine and allow you to do everything the other one does so don’t stress too much about this decision.
* (probably) When creating environments, create at least one python 2 environment using the “python=2” parameter. It is up to you whether you want to work in Python 2.x or Python 3.x. For the moment, Anubhav prefers 2.x backward-compatible code for most base libraries (e.g. FireWorks), so if you use 3.x as your main environment, make sure your syntax doesn’t depend on the newer features. See next bullet point for more.
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

### Configure Pycharm IDE
An integrated development environment (IDE) allows you to be a much more productive coder. It is like a text editor but contains many useful keyboard shortcuts, code-completion tools, refactoring tools, and debugging/profiling tools to help you be more productive. It can also automatically reformat your code to trim line lengths and add proper whitespace to be in-line with recommended Python formatting guidelines.

[Pycharm](https://www.jetbrains.com/pycharm/) is the group’s recommended IDE for Python and they offer a free community edition (CE) that contains all the features you need. There are other programs you might consider like Sublime Text, although those are more like advanced text editors than proper IDEs. Note that there are some advanced programmers that know their way around an IDE but still prefer an editor like vi or emacs with appropriate plugins. This is fine so long as (i) you are an advanced programmer and (ii) you have first tried an IDE for a few months and really tried to make use of it, but find that it hampers your productivity. Note: 1. You can add vim and emacs bindings to Pycharm and 2. most people that think they fall into the advanced category do not and find themselves making mistakes that could easily be avoided with an IDE. Thus, it is recommended that essentially everyone in the group use an IDE.

After downloading and installing Pycharm and launching it for the first time, you’ll be asked some options. I suggest using the default Mac OS X keymap unless you are already very familiar with Emacs or other keymaps. Note that these things can be changed later if desired.

Next, and assuming you’ve already cloned the source code of your desired repos from Github, you do the following:
1. Create a new project (give it any name)
2. In the menu bar, click Pycharm CE -> Preferences -> Project -> Project interpreter. Change this to your conda Python interpreter. You can also set your default interpreter from File->Default Settings->Project Interpreter and set that to your conda environment interpreter. Do not skip these steps! 
3. Click File->Open and navigate to the root folder of one your desired repos (I suggest pymatgen to start) and click open.
4. Make sure to select “open in current window” AND check “add to currently opened projects”
5. Repeat step #3 for all your desired codebases. When finished, you should see in the sidebar all the various codebases.

If you want to try adding some of your own scratch code, then:
1. Navigate in the sidebar to your main project folder (folder with your chosen project name). Right-click and click New->Python package. Give it a name.
2. The previous command created a new folder. Navigate inside that folder, right-click, and choose New->Python file. When finished, that folder should contain two .py files - __init__.py (created automatically for the new Python package) and your chosen filename.
3. Finally, type some code in your new file. It can be simple as print("hello world"). To execute the code, you can use Ctrl+Shift+R with default Mac keybindings or go through the “Run” menu for more options.

Now you are all set! There are many things you might want to do:
* Configure the way your Pycharm window looks. For large monitors, Anubhav likes Project Navigation at left, main code in center, and “Structure” panel on right which is basically like an outline of a particular Python file showing all the classes, functions, etc. at a glance.
* Explore the various options and capabilities of PyCharm. Appendix D of the handbook has some tips and you can enable PyCharm’s “tip of the day” which will really step you through some of the great features. 

### Other things to do
* Set up your Time Machine backup (make sure you have purchased or received an external hard disk).
https://support.apple.com/en-us/HT204412
* You can also set up an online backup plan (e.g., Crashplan or Backblaze) to provide you with a second backup.
* Install MongoDB.
* Install Docker.
* Purchase Microsoft office from LBNL software distribution.

-----------------------------------------------------------------------------------------------------------------------------

## Software help groups <a name="softwaresupport"></a>
If you have problems with software, and in particular the software maintained by our group and our collaborators, you should contact the appropriate help group. The documentation for the software will list what that channel is; if not, try the Github Issues page. If you are reaching out for help, try to provide everything needed to quickly reproduce and debug the problem (files, test code, etc).

Two other ways to get software help that are more self-guided are:
* If you are having trouble using a particular class or function, look for unit tests within the code, which often demonstrate how to use the class or function
* If the class or function has a unique name (e.g., MaterialsProjectCompatibility), another option is to both Google and search on github.com for the particular class/function. The github.com search will often reveal code snippets from users all around the world. 

-----------------------------------------------------------------------------------------------------------------------------

## Computing Resources <a name="computing"></a>
Our group’s main computing resources are:
* [NERSC](#nersc) (the LBNL supercomputing center, one of the biggest in the world)
* Peregrine (the NREL supercomputing center)
* Lawrencium / Berkeley Research Computing
* Argonne Leadership Computing Facility(sometimes)
* Oak Ridge Leadership Computing Facility (sometimes)

At any time, if you feel you are computing-limited, please contact Kristin so she can work with you on finding solutions.

### NERSC <a name="nersc"></a>

#### To get started with calculations at NERSC: <a name="startnersc"></a>
1. Ask Kristin about whether you will be running at NERSC and, if so, under what account / repository to charge.
2. Request a NERSC account through the NERSC homepage (Google “NERSC account request”).
3. Someone at NERSC will validate your account and assign you computing hours
4. At this point, you should be able to log in, check CPU-hour balances, etc. through “NERSC NIM” and “My NERSC” portals
5. In order to log in and run jobs on the various machines at NERSC, review the NERSC documentation.
6. In order to load and submit scripts for various codes (VASP, ABINIT, Quantum Espresso), NERSC has lots of information to help. Try Google, e.g. “NERSC VASP”.
...* Note that for commercial codes such as VASP, there is an online form that allows you to enter your VASP license, which NERSC will confirm and then allow you access to.
7. Please make a folder inside your project directory and submit all your jobs there as your home folder has only about 40GB of space. For example, for matgen project, your work folder path should be something like the following:
`/global/project/projectdirs/matgen/YOUR_NERSC_USERNAME`
8. You can also request a database for your project to be hosted on NERSC. Google “MongoDB on NERSC” for instructions. Donny Winston or Patrick Huck can also help you get set up and provide you with a preconfigured database suited for running Materials Project style workflows.

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
alias cori="ssh your_username@cori.nersc.gov
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
You also specify the resource type in your batch file. 

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


### Peregrine <a name="nrel"></a>


### Additional resources:
Other Persson group members and the NERSC website are both excellent resources for getting additional help. If that fails, you can reach out to the NERSC Operations staff:
* 1-800-666-3772 (or 1-510-486-8600)
* Computer Operations = menu option 1 (24/7)
* Account Support = menu option 2,  accounts@nersc.gov
* HPC Consulting = menu option 3, or consult@nersc.gov
* Online Help Desk = help.nersc.gov

-----------------------------------------------------------------------------------------------------------------------------

## Our software stack
A brief summary of our software stack includes:
* pymatgen / pymatgen-db - for representing and analyzing crystal structures, as well as setting up/performing manual calculations
* FireWorks - for executing and managing calculation workflows at supercomputing centers
* custodian - instead of directly running an executable like VASP, one can wrap the executable in custodian to detect and fix errors
* atomate - for quickly defining multiple types of materials science workflows
* matminer - for large data analysis and visualization

We also heavily use the Materials Project database.

To learn how to use the software stack, you can consult the documentation of the individual codebases as well as review the following resources:
* The 2018 Materials Project workshop (note that MatMethods is now called atomate): https://github.com/materialsproject/workshop-2018
* The 2014 Materials Virtual Lab presentations: 
https://materialsvirtuallab.org/software/
* The Materials Project YouTube tutorials: 
https://www.youtube.com/user/MaterialsProject

If you have a specific question, sometimes the easiest solution is to post it to the Slack group and crowdsource the answer (or just ask Shyam). 

-----------------------------------------------------------------------------------------------------------------------------

## Materials Science

> “Don’t despair of standard dull textbooks. Just close the book once in awhile and think what they just said in your own terms as a revelation of the spirit and wonder of nature”.
>
> \- Richard Feynman

It can be difficult to find resources that explain concepts in materials science clearly. Often, struggling through multiple attempts to understand a topic using several different resources in a patchwork and non-linear fashion is the only way forward. That said, the resources listed below are particularly helpful.

### Density functional theory
For beginners to density functional theory, I would recommend the book “Density Functional Theory: A Practical Introduction”, which truly achieves what it states by providing physical insights and relevant information rather than just list equations. Copies are available within the group.

If you are interested to explore applications of density functional theory, you might try the E-book from Professor John Kitchin:
https://github.com/jkitchin/dft-book
*Note that this book has chosen to use the Atomic Simulation Environment (ASE) to set up simulations rather than the pymatgen code that we prefer, but that is a minor point.*

Finally, for specific calculations with VASP, there are resources online from a 2016 workshop conducted at LBNL, including videos and training materials:
http://www.nersc.gov/users/training/events/3-day-vasp-workshop/
http://cms.mpi.univie.ac.at/wiki/index.php/NERSC_Berkeley_2016

### General materials science topics
To gain a quick introduction to many topics in materials science, you might try the (horribly-named) web site from the University of Cambridge: Dissemination of IT for the Promotion of Materials Science (DoITPoMS):
https://www.doitpoms.ac.uk

The explanations in this site are very basic, but what they do cover is well-explained and incorporates helpful visuals. Although you won’t ever master a topic from this site, it is often a good starting point that can help you unlock a more intermediate resource.

There are also some nice chapters in the following e-book:
https://en.wikibooks.org/wiki/Introduction_to_Inorganic_Chemistry
For example, Chapter 5 has a nice rundown of common crystal structures.

### Online tools
A nice tool for visualizing phonon modes is: http://henriquemiranda.github.io/phononwebsite/phonon.html

-----------------------------------------------------------------------------------------------------------------------------

## Computer programming
Note that there are usually many excellent resources to choose from when learning computer science topics. You usually have the flexibility of choosing to learn from a book, a video series, or even interactive tutorials like www.learnpython.org. Use the list below as potential starting points, but there exist many other high-quality alternatives you can find on your own and may be even better-suited to your needs.

### Python
For pure beginners to Python, you might try the book “Head First Python”. It is a fun and easy introduction to Python. Beginners that know a little but not a lot of Python can also look at “How to Make Mistakes in Python” (ebook). For intermediate programmers, you might try “20 Python Libraries You Aren’t Using (But Should)”  (ebook). For advanced programmers, you might try “Expert Python Programming”.

### Data mining and Data Analysis
For learning basic data mining libraries (pandas, scikit-learn) as well as some skills like using git and Github, you might try the online YouTube videos from Kevin Markham, an educator at Data School. These videos also do a good job of pointing you to supplementary material:
https://www.youtube.com/user/dataschool
https://github.com/justmarkham

You might also try the book “Python for Data Science For Dummies” (please note: this is different than “Data Science for Dummies”).

For a more materials-centric view, you can try working your way through the Machine Learning In Materials tutorial in the Appendix of this handbook.

A very comprehensive set of suggestions for further resources is listed here:
http://bit.ly/2jHXIVJ

### MongoDb
A (now somewhat old, but still clear) resource for beginning to use MongoDb is the “The Little MongoDB Book”:
https://github.com/karlseguin/the-little-mongodb-book
There is also an extensive library of webinars on MongoDb on their official web site.

