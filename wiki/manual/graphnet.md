# Introduction
Logan Grosenick et al developed the Graph-constrained Elastic-Net regression penalty as a way of handling regression problems where the solution is expected to be both sparse and spatially-temporally structured (i.e., your significant voxels should be clustered together in space and across trs of an experiment). Essentially using graphnet penalty means using an l1, l2 and clustering penalties added together in your objective function. By doing some additional math, Logan was able to construct an SVM using the graphnet penalty. Usually when Brian talks about fitting Graphnet, he is talking about the svm-with-graphnet penalty. 
For more information, read the [graphnet manuscript](https://arxiv.org/abs/1110.4139) and this [lab paper](https://www.ncbi.nlm.nih.gov/pubmed/23298747).

Logan, with some help from lab alumnus Kiefer Katovich, wrote most of the code for graphnet around 2012, though it hasn't always been functional in the lab for the last few years since we lost the developers. 

This guide will walk you through the process of installing graphnet and running it on some sample data. 

# Installation

```
git clone https://github.com/spanlab/neuroparser.git
cd neuroparser
virtualenv npvenv (on sherlock follow the instructions [here](http://sherlock.stanford.edu/mediawiki/index.php/Python#using_a_fresh_virtualenv)
source npvenv/bin/activate
pip install pip_requirements.txt
python setup.py install
```

## On sherlockml python
```
easy_install --user pip
python -m pip install --user --upgrade pip
python -m pip install --user --upgrade virtualenv
python -m virtualenv venv
source venv/bin/activate
pip install -r pip_requirements.txt
python setup.py install
```

# Usage 


Once you've done that, go ahead and check out graphnet_sa/example_runner.py for an example script. 

You will need your data in the format:

    test_data/
    |-- jk160415
    |   |-- buy_vs_not_onset.1D
    |   |-- stock_mbnf.nii
    |-- jr160430
    |   |-- buy_vs_not_onset.1D
    |   |-- stock_mbnf.nii
    |-- jr160501
    |   |-- buy_vs_not_onset.1D
    |   |-- stock_mbnf.nii
    |-- tt29.nii
    '''
    
 More to come!
