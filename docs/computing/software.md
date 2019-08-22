## Our software stack
A brief summary of our software stack includes:

* [pymatgen](https://github.com/materialsproject/pymatgen) / [pymatgen-db](https://github.com/materialsproject/pymatgen-db) - for representing and analyzing crystal structures, as well as setting up/performing manual calculations

* [FireWorks](https://github.com/materialsproject/fireworks) - for executing and managing calculation workflows at supercomputing centers

* [custodian](https://github.com/materialsproject/custodian) - instead of directly running an executable like VASP, one can wrap the executable in custodian to detect and fix errors

* [atomate](https://github.com/hackingmaterials/atomate) - for quickly defining multiple types of materials science workflows

* [matminer](https://github.com/hackingmaterials/matminer) - for large data analysis and visualization

We also heavily use the Materials Project database.

------------------

Additional software used by many members of our group include:

* [VESTA](http://jp-minerals.org/vesta/en/) -

* [Ovito](https://www.ovito.org/) -

* [VMD](https://www.ks.uiuc.edu/Research/vmd/) -

* [Studio 3T](https://studio3t.com/download/) -

* [PyCharm](https://www.jetbrains.com/pycharm/) -

* [iTerm2](https://www.iterm2.com/) -

------------------

To learn how to use the software stack, you can consult the documentation of the individual codebases as well as review the following resources:

* The 2018 Materials Project workshop (note that MatMethods is now called atomate): [https://github.com/materialsproject/workshop-2018](https://github.com/materialsproject/workshop-2018)

* The 2014 Materials Virtual Lab presentations:
[https://materialsvirtuallab.org/software/](https://materialsvirtuallab.org/software/)

* The Materials Project YouTube tutorials:
[https://www.youtube.com/user/MaterialsProject](https://www.youtube.com/user/MaterialsProject)

If you have a specific question, sometimes the easiest solution is to post it to the Slack group and crowdsource the answer (or just ask Shyam).

---

## Software help groups <a name="softwaresupport"></a>
If you have problems with software, and in particular the software maintained by our group and our collaborators, you should contact the appropriate help group. The documentation for the software will list what that channel is; if not, try the Github Issues page. If you are reaching out for help, try to provide everything needed to quickly reproduce and debug the problem (files, test code, etc).

Two other ways to get software help that are more self-guided are:
* If you are having trouble using a particular class or function, look for unit tests within the code, which often demonstrate how to use the class or function
* If the class or function has a unique name (e.g., MaterialsProjectCompatibility), another option is to both Google and search on github.com for the particular class/function. The github.com search will often reveal code snippets from users all around the world.

---

Authors: John Dagdelen, Eric Sivonxay

Contact: jdagdelen@lbl.gov
