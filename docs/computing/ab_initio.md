# VASP
We generally run 2 versions of VASP - 5.4.4 and 6 (in beta). VASP 6 is used primarily on nodes operating on the Knights Landing (KNL) CPU architecture, while VASP 5.4.4 is generally used on all other CPUs.

### VASP on NERSC
NERSC maintains compiled, and optimized versions of the vasp binaries. To get access to these binaries, you must be added to the group VASP license. If/when you determine that your research requires the use of VASP, contact Jimmy Shen and Eric Sivonxay.

The potcar directory is:
```
/project/projectdirs/matgen/POTCARs
```

### VASP on Berkeley Research Computing (savio)
When running vasp on savio, we have access to our own compilations. To access the vasp binaries on savio, add the following line to your .bashrc file.

```
export MODULEPATH=${MODULEPATH}:/global/home/groups/co_lsdi/sl7/modfiles
```

POTCARs are located at:
```
/clusterfs/cloudcuckoo/POTCARs
```

For issues with these compilations, please contact Eric Sivonxay (esivonxay@lbl.gov)

### Lawrencium
When running vasp on Lawrencium, we have access to our own compilations. To access the vasp binaries on Lawrencium, add the following line to your .bashrc file.

```
export MODULEPATH=${MODULEPATH}:/clusterfs/mp/temp_modfiles
```

POTCARs are located at:
```
/clusterfs/mp/software/POTCARs
```

For issues with these compilations, please contact Eric Sivonxay (esivonxay@lbl.gov)

# Gaussian
Gaussian is available on Savio. Anyone wanting to use Gaussian on Savio must sign [Exhibit A](https://drive.google.com/file/d/1Tg0xb5Nuet8HByJx3nWo7lBEIzjo5seI/view?usp=sharing). Print out two copies of the form and sign both (i.e. they should not be signed electronically). Then provide both physical copies to Alice. If you are remote, an e-mailed copy will suffice for the time being. 

# Q-Chem
More Information to come

---
Authors: Eric Sivonxay

Contact: esivonxay@lbl.gov
