# EstTools
Standalone scripts for background estimation. I typically run this within CMSSW on the LPC cluster. 

## Set up CMSSW

```
tcsh
source /cvmfs/cms.cern.ch/cmsset_default.csh
setenv SCRAM_ARCH slc6_amd64_gcc700
cmsrel CMSSW_10_2_22 
cd CMSSW_10_2_22/src/
cmsenv
```

## Get code
```
git clone git@github.com:mkilpatr/HGCTools.git
git submodule init
git submodule update --recusive
```

## How to run the plotting methods

```
cd HGCStudies
python3 ThermalCyclePlotter.py -d <Location where file is stored> -t Deformation
cd ../hexmap
python3 plot_summary.py <location where json file is including filename> -l <label you would like on the plot> -p <Test Type>
```

## How to run the Module tolerances

The module tolerances are set to run the full 2D simulation on Condor on the FNAL LPC cluster. Some changes can certainly be made to get it to work on POD. The following will split the desired testing parameters into different jobs and run them in parallel.
```
./process.py -p . -m ModuleStudies.C -o ModuleTolerances_300K_31Jan21_baseplateCenters_Full -l . -b diffTolerances.conf
source submit.sh
```

Then you need to run the ModuleAnalysis function to combine the results that are located on EOS and it saves the results locally. Notice that the functions takes as input the directory name of where the files are stored on EOS.
```
root -l
.L ModuleStudies.C+
ModuleAnalysis("ModuleTolerances_300K_31Jan21_baseplateCenters_Full")
```

## Fiducials QC
The fiducials QC is a Jupyter notebook that takes in the excel files from the OGP and computes the average height with max/min values, the center placement of the components and the rotations. You will need to go to the location on the PC and run:
```
jupyter notebook
```
