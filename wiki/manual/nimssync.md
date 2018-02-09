# Syncing Data from NIMS

One of the most common tasks we must perform in analyzing fMRI data is moving data around. 

The naive way of syncing data from NIMS to your local machine to conduct local analyses is to copy files individually from the [NIMS website](https://cni.stanford.edu/nims/). This is reasonably simple, but it takes some time to manually move all of these files around into the format we'll want to use for data analysis. 

With that in mind, NB wrote a small nims sync utility that should help you get your data copied over much faster. The rest of this entry describes how to set up your data for performing one of these fast syncs. 

# Instructions

## CNI containers
Basically every scan ever done at CNI is placed on multiply redundant servers down in the CNI server room and elsewhere. Copying that data over is reasonably easy if we know what computer to connect to. CNI-containers are artifical computer interfaces that you can connect to in order to access the data as if you were on a local drive. There is a CNI container specifically for Knutson lab which is already set up. To get access to it, you will need to ask Michael Perry to give you access to cnic18.stanford.edu. If you would like to learn more about them, check out [this page](https://cni.stanford.edu/wiki/LXC).

Once you've gotten access and have logged into the container, you should be able to use normal unix utilities like rsync and scp to copy these files around. The nims sync script is designed to automate a great deal of this for you. 

## Setting up a subjects.csv file
Data stored on the containers is labelled by scan date and time, so in order to copy your data over you need to know the mapping from subject to folder. 

For example, we may want to get the following subjects in a particular scan directory (where you save your data to, see [scanning](scanning.md)). 
```
20180120_1625_16817: kj012018
20180120_1744_16818: cg012018
20180123_1648_16834: cb012318
20180123_1825_16836: al012318
```
To find these numbers, you can log into NIMS and look at an individual scan's description by double clicking on the session and looking at the timestamp. Exam `16859` with timestamp `2018-01-26 12:50:21` will be converted to `20180126_1250_16859`. 

By keeping track of these numbers as you collect scans and recording them, we can always keep an up to date record of _exactly_ where the corresponding data will be kept. Note that if you end and start a new session, there will be two folders for each of these directories. 


Once you have them, you'll want to keep a csv file (we'll call it `subjects.csv`) with these columns:
```
Folder,Subject,...
20180120_1625_16817,kj012018,...
20180120_1744_16818,cg012018,...
20180123_1648_16834,cb012318,...
20180123_1825_16836,al012318,...
```

## Usage of nims_sync.py

To sync, we can then use the `nims_sync.py` script in `spantoolbox/utilities`. 

```
python nims_sync.py  --user nickborg --experiment stockmri2 --sfile subjects.csv --dest /path/to/where/i/want/files
```

* user will be your SUID
* experiment will be the experiment name of the folder where data is saved on NIMS. 
* If you need to gather data from another PIs server, you would use --PI your-pi-name. 
* to see which folders will be copied from the server without writing anything to disk, use the option --dry
* call python nims_sync.py --help

(Note that nims_sync.py will need to be in your current directory, or you'll need to provide a path to it, or you'll need to have it added to the system path)

## Putting your data into subject folders. 

Once you've synced your local copy of the data, you'll probably want to move them to folders names after subjects, and to give reasonable names to the scan files to make them easier to work with. 

With that in mind, you'll probably want to use the `spantoolbox/utilities/place_data` script. 


This well require two other things:
* a project_spec file:
You'll want to make a file like the following:

filename: project_spec
location: ~/.../natgeo/
Contents:
```
project = 'natgeo'
tasks = ['instagram','campaign']
scans = ['T1w_9mm_BRAVO', 'Instagram', 'Campaign']
scan_names = ['anat', 'instagram', 'campaign']
behaviorals = ['instagram.csv', 'campaign.csv']
```
Suppose that we get folders named `15576_5_1_Instagram_EPI_29mm_2sec` and `15576_5_1_Campaign_EPI_29mm_2sec`, but we want to rename the corresponding files `instagram.nii` and `campaign.nii`. 

Then `scans` will contain Instagram and Campaign as those strings are in the scan names, and `scan_names` will contain the names we want for those. 

`tasks` will refer to the higher level abstraction of whether a subject did a task at all (in the case where there are two scans within a given task, this might matter. 

* you'll need your subject file to look like this:
```
Folder,Subject,Researcher,task_instagram,task_campaign,T1w_9mm_BRAVO,Instagram,Campaign,...
20170726_1454_15569,kg072617,NS,1,1,1,1,1,...
20170727_1642_15576,cm072717,NS,1,1,1,1,1,...
Running `python status.py` and then `python place_data.py` in a folder `utilities` copied from spantoolbox would make this easy. 
```
(Here there's a 1 if a subject had a particular scan, otherwise you'll get a zero.)




If more than one scan for a subject has `Instagram` in the title, you should get a bash prompt asking which of the two files you'll want to use (usually the later, larger one). 
 
Similarly, the `qa.py` script will make qa significantly faster .

```
Note from nb: these scripts right now are a mess, and I wanted to totally revamp them to make them not bad, but at the moment they do what they are meant to do. If you end up trying to use them and get stuck, email me and i'll see what i can do. 
```

