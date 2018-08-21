# Using PyParanoid on Cedar



### logging into Graham or Cedar


```
ssh yourname@cedar.computecanada.ca
```


### setting up virtual environment

relevant links:
+ https://www.westgrid.ca/events/machine_learning_using_jupyter_notebooks_graham
+ https://docs.computecanada.ca/wiki/Jupyter
+ https://docs.computecanada.ca/wiki/SSH_tunnelling
+ https://docs.computecanada.ca/wiki/Project_layout

run below commands once to set up Jupyter environment
```
module load python/2.7
virtualenv $HOME/jupy2
source jupy2/bin/activate
pip install jupyter
pip install pyparanoid
echo -e '#!/bin/bash\nunset XDG_RUNTIME_DIR\njupyter notebook --ip $(hostname -f) --no-browser' > $VIRTUAL_ENV/bin/notebook.sh
chmod u+x $VIRTUAL_ENV/bin/notebook.sh
```

on local system (for UNIX)
```
sshuttle --dns -Nr yourname@cedar.computecanada.ca
```

running Jupyter notebook
```
salloc --time=1:0:0 --ntasks=1 --cpus-per-task=2 --mem-per-cpu=1024M --account=def-haney srun $VIRTUAL_ENV/bin/notebook.sh
```

### miscellaneous server commands

```
diskusage_report
```

### other code packages

```
git clone https://github.com/ryanmelnyk/PhyloTools.git
git clone https://github.com/ryanmelnyk/GenomeAssembly.git
```

### Building a tree

```
module load fasttree
```

### comparative genomics

Check out the python notebooks at https://github.com/ryanmelnyk/PyParanoid/tree/master/analysis_ipython

### Assembling a genome?
