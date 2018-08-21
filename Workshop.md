# Bioinformatics bootcamp setup


### for Windows users

install [Conda](https://www.anaconda.com/download/) - pick version 2.7


### logging into Cedar

```
ssh yourname@cedar.computecanada.ca
```

### downloading Pseudo PyP database
size is about 13 gigabytes
```
rsync -chavzP --no-p --no-g --stats yourname@graham.computecanada.ca:projects/def-haney/data/Pseudo /path/to/your_local_folder
```

### downloading alignments for tree-making
about 1 gigabyte
```
rsync -chavzP --no-p --no-g --stats yourname@graham.computecanada.ca:projects/def-haney/data/orthos /path/to/your_local_folder
```

### installing software

```
pip install jupyter
pip install pyparanoid
brew tap brewsci/bio
brew install FastTree
git clone https://github.com/ryanmelnyk/PhyloTools.git
git clone https://github.com/ryanmelnyk/GenomeAssembly.git
```

### other software

for tree viewing:
http://dendroscope.org/
http://tree.bio.ed.ac.uk/software/figtree/
