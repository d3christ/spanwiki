# Introduction

[Sherlock](http://sherlock.stanford.edu/mediawiki/index.php/Main_Page) is Stanford's School of Humanities and Sciences's very own HPC, and a big help to us. 

Often when we are running computationally expensive analyses (like svmrfe, for example), running a leave-one-subject-out cross validation procedure might take up something like 20-60gb of RAM (more than we have on local computers) and might take a very long time to run. 

Sherlock lets us run jobs that require larger amounts of memory than we might have locally, and it lets us run many such jobs at the same time. 

This parallelism can really speed things up. For example, if we have N subjects, in LOO CV, we will need to train N svms for any given set of parameters. But there is no reason that these SVMS have to be trained in any order--they are all independent of eachother, and if we have enough computers, we can run them all at the same time. 

# Set up
The first thing you'll need to to is follow the first three steps of the [Getting started](http://sherlock.stanford.edu/mediawiki/index.php/Main_Page#Getting_Started) section of the Sherlock wiki. This will get you logged in. If you don't currently have access, then you must ask Brian to have Kilian Cavalotti <research-computing-support@stanford.edu> add you to the Knutson lab group. 

# Keeping data

See the sherlock wiki for all the different places to keep data. Normally, though, we keep data that we'll want other members of the lab to have access to on `$PI_SCRATCH=/scratch/PI/knutson`. 

# Submitting jobs
Submitting a job is done through sbatch files. Read more about these files [here](http://sherlock.stanford.edu/mediawiki/index.php/SLURMSubmit). 

# Using python virtual environments
Because of the way sherlock works, every job that you run on sherlock must begin with some specification for which software you'll need for that. If you're using python (as we do for svmrfe) then you'll want to use a python virtual environment for package management. For SVMRFE you can use one of the existing lab environments. Otherwise, you should be free to use your own. See [this section](http://sherlock.stanford.edu/mediawiki/index.php/Python#using_a_fresh_virtualenv) for more information.

# The sbatch_maker script
If you need to programmatically generate sbatch scripts (for example, one for each subject), you can use the handy sbatch maker script included below. 

The script assumes that I have some python script (it could also be a bash script, or whatever else you run from a CLI) that takes a command line argument to specify the job. In this case, it takes a subject's ID to specify which subject will be left out as the test subject. 

```python
import os

################## USER PARAMS #####################
SUBJECT_FILE = '/scratch/PI/knutson/cuesvm/cue_patients_subjects.txt'
JOB_STRING   = 'cue_relapseds'
MEMORY       = '32gb'
TIME         = '48:00:00'
NODES        =  '1'
QOS          = 'normal'#must be 'long' if TIME > than 48 hours
EXEC_DIR     = "/scratch/PI/knutson/cuesvm/svmrfe/"
EXECUTABLE   = 'srun python run_sgdrfe_relapse.py' # takes subject as argv[1]
VENV         = '/scratch/PI/knutson/cuesvm/svmrfe/venv'
################## END PARAMS #####################


def make_batch(subject):
    sbatch = [
          "#!/bin/bash",
          "#",
          "",
          "#all commands that start with SBATCH contain commands that are just  used by SLURM for scheduling",
          "#################",
          "#set a job name",
          "#SBATCH --job-name=" + subject + JOB_STRING,
          "#################",
          "#a file for job output, you can check job progress",
          "#SBATCH --output=" + subject + JOB_STRING + ".out",
          "#################",
          "# a file for errors from the job",
          "#SBATCH --error=" + subject + JOB_STRING + ".err",
          "#################",
          "#time you think you need; default is one hour",
          "#in minutes in this case",
          "#SBATCH --time=" + TIME,
          "#################",
          "#quality of service; think of it as job priority",
          "#SBATCH --qos=" + QOS,
          "#################",
          "#number of nodes you are requesting",
          "#SBATCH --nodes=" + NODES,
          "#################",
          "#memory per node; default is 4000 MB per CPU",
          "#SBATCH --mem=" + MEMORY,
          "#you could use --mem-per-cpu; they mean what we are calling cores",
          "#################",
          "#tasks to run per node; a \"task\" is usually mapped to a MPI  processes.",
          "# for local parallelism (OpenMP or threads),use \"--ntasks-per-node=1 --cpus-per-tasks=16\" instead",
          "",
          "#################",
          "",
          "#now run normal batch commands",
          "ml load python/2.7.5",
          "cd " + EXEC_DIR,
          "source " + VENV,
          EXECUTABLE + ' '  + subject,
    ]

    fname = JOB_STRING + '_' +  subject + ".sbatch"

    with open(fname, 'w') as f:
        for i, line in enumerate(sbatch):
            f.write(line + '\n')
    return fname

if __name__ == "__main__":

    with open(SUBJECT_FILE, 'r') as f:
        subjects = [x for x in f.read().split('\n') if len(x) == 8]

    for s in subjects:
        f = make_batch(s)
        os.system('sbatch ' + f)
```
