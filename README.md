# grrm2xtb

## Abstract
This is a Python3 script to use Grimme's XTB [https://github.com/grimme-lab/xtb] as a QM engine for GRRM17.
While we tried several MIN, SADDLE, SC-AFIR, and MC-AFIR jobs and this script seems to reasonably work, we provide *NO guarantee*.

## Software version
We use this script with GRRM17, xtb-6.3.3, and Python 3.6.7/3.6.8. Other versions have not been tested.

## Settings

 - Set environmental variables and PATH for XTB. XTB should be able to run with a command `xtb`. 
 - Copy this python script with an appropriate name where PATH is set. This script should be able to run via its file name, not its full path.
 - The shebang `#!/usr/bin/python3` needs to be modified into your Python3 interpreter.

## GRRM Job and XTB calculation settings

`%link=non-supported` should be in the first line and `sublink=script_name` should be in the options section in your GRRM input.
In the `#` section, simply write a job type. The following is a MIN job.
```
%link=non-supported
#MIN

0 1
C 0.000 0.000 0.000
C 1.200 0.000 0.000
....
Options
...
sublink=grrm2xtb
```

The calculation settings for XTB (charge, spin multiplicity, solvation, XTB version) need to be given as environmental variables (see below), NOT in the GRRM input.
The charge and multiplicity in the GRRM input are neglected and have no effect.
In case no environmental variables are set, the default settings are used (charge = 0, multiplicity = 1, gas phase, GFN2).
`XTB_MULTI` should be a spin multiplicity like in Gaussian or GRRM, not Na-Nb (--uhf for XTB). Internally, `--uhf` is set to multiplicity-1. 

```
export XTB_CHARGE=0
export XTB_MULTI=1
export XTB_SOLVENT=CH2Cl2
export XTB_PARAM=2
```

## Parallelization

How many cores are used in each XTB job can be set as a standard way for XTB [https://xtb-docs.readthedocs.io/en/latest/setup.html#environment-variables-for-xtb].

In AFIR or other multi-process jobs in GRRM, these variables should be appropriately set.
If you set `OMP_NUM_THREADS=4,1` and run GRRM with 4 parallel processes, 4\*4=16 cpus will be used.
Generally, XTB calculations are very fast for small organic and organometallic compounds (less than 100-200 atoms),
setting `OMP_NUM_THREADS=1,1` and increasing the GRRM processes as many as possible may be a good way for SC-AFIR and MC-AFIR search.

## Run GRRM17

After setting the all required envirionmental variables, run GRRM as usual.

*Caution:* In the output, DIPOLE,POLARIZABILITY, and S\**2 are always 0. This script is just for a rough search for structures.

