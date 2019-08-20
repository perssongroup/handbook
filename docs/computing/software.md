## Our software stack
A brief summary of our software stack includes:

* [pymatgen](https://github.com/materialsproject/pymatgen) / [pymatgen-db](https://github.com/materialsproject/pymatgen-db) - for representing and analyzing crystal structures, as well as setting up/performing manual calculations

* [FireWorks](https://github.com/materialsproject/fireworks) - for executing and managing calculation workflows at supercomputing centers

* [custodian](https://github.com/materialsproject/custodian) - instead of directly running an executable like VASP, one can wrap the executable in custodian to detect and fix errors

* [atomate](https://github.com/hackingmaterials/atomate) - for quickly defining multiple types of materials science workflows

* matminer - for large data analysis and visualization

We also heavily use the Materials Project database.

------------------

To learn how to use the software stack, you can consult the documentation of the individual codebases as well as review the following resources:

* The 2018 Materials Project workshop (note that MatMethods is now called atomate): https://github.com/materialsproject/workshop-2018

* The 2014 Materials Virtual Lab presentations:
https://materialsvirtuallab.org/software/

* The Materials Project YouTube tutorials:
https://www.youtube.com/user/MaterialsProject

If you have a specific question, sometimes the easiest solution is to post it to the Slack group and crowdsource the answer (or just ask Shyam).
