# Syncing Data from NIMS

One of the most common tasks we must perform in analyzing fMRI data is moving data around. 

The naive way of syncing data from NIMS to your local machine to conduct local analyses is to copy files individually from the [NIMS website](https://cni.stanford.edu/nims/). This is reasonably simple, but it takes some time to manually move all of these files around into the format we'll want to use for data analysis. 

With that in mind, NB wrote a small nims sync utility that should help you get your data copied over much faster. The rest of this entry describes how to set up your data for performing one of these fast syncs. 

# Instructions

## CNI containers
Basically every scan ever done at cni is placed on multiply redundant servers down in the CNI server room and elsewhere. Copying that data over is reasonably easy if we know what computer to connect to. CNI-containers are artifical computer interfaces that you can connect to in order to access the data as if you were on a local drive. There is a CNI container specifically for Knutson lab which is already set up. To get access to it, you will need to ask Michael Perry to give you access to cnic18.stanford.edu. If you would like to learn more about them, check out [this page](https://cni.stanford.edu/wiki/LXC).

Once you've logged into the container, you should be able to use normal unix utilities like rsync and scp to copy these files around. The nims sync script is designed to automate a great deal of this for you. 

