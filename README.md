# argon2-for-zfs
This script does the following for ZFS users who want Argon2 as the key derivation algorithim
- Colect and store Argon2 parameters (salt, thread count, memory, iterations)
- Generate a hex key from the password.
- Automatically load the key for the dataset
- Aumotaically mount the dataset
- Automatically run any desired scripts after a dataset is mounted.

## Recommendations
- Back up the argpass.cfg file after creation. It contains the salt and other parameters that will be needed to turn your password into a key. 
- If you are using raidz, it is a good practice to put the configuration and scripts in their own dataset on the same pool as the datasets you are encrypting.
- It's a good idea to set the dataset with these scripts to read-only.
```
zfs set readonly=on pool/dataset
```

## Installation
```
git clone https://github.com/jamespeters86/argon2-for-zfs.git
```

## Setup
Run 
```
./argpass -s
```
If the argon2 package is not installed, it will attempt to install it via apt. 
Setup will allow you to tune paramters and setup your datashare. 

Setup can be rerun with -s to add attional datasets at any time. 

## Dataset Creation
If you did not create a dataset durring setup you can manaully generate the hex key with the -p option.

Use "-o key-format=hex" with zfs create.

If you do not want to ever print the hash to the screen, you can pipe the last line of output to zfs create.
```
./argpass -p | tail -1 |  zfs create -o encryption=on -o keyformat=hex ...
```

## Dataset Loading
```
./argpass
```
or
```
./argpass poolname/dataset
```
If no dataset is specificed, it will use the paramters for the first dataset listed in the file.

## Automated scripts
To have scripts run automatically after a dataset is mounted, place them in argpass.start.d/poolname.datasetname. The scripts will be execuated in numerical order so they can be numbered to meet dependancies. For example 00-bind-mount, 01-apache. Sample scripts are provided in sample.d.
