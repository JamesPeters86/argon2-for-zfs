# argon2-for-zfs
This script does the following for ZFS users who want Argon2 as the key derivation algorithim
- Colect and store Argon2 parameters (salt, thread count, memory, iterations)
- Generate a hex from the password.
- Automatically load the key for the volume
- Aumotaically mount the volume
- Automatically run any desired scripts after a volume is mounted.

## Installation
```
git clone https://github.com/jamespeters86/argon2-for-zfs.git
```

## Setup
Run 
```
./argpass -s
```
If the argon2 package is not installed, it will attempt to install it via apt. It will then ask you for thread count, memory usage, and iterations. It will time test those settings. It can be rerun with -s to add attional volumes. 

## Volume creation
You can generate a hash in one of two ways:
- At the end of the setup process
- With the -p option 

Use "-o key-format=hex" with zfs create.

If you do not want to ever print the hash to the screen, you can use the -p option and pipe the last line of output to zfs create.
```
./argpass -p | tail -1 |  zfs create -o encryption=on -o keyformat=hex ...
```

## Volume Loading
```
./argpass
```
or
```
./argpass poolname/volumename
```
If no volume is specificed, it will use the paramters for the first volume listed in the file.

## Automated scripts
To have scripts run automatically after a volume is mounted, place them in argpass.start.d/poolname.volumename. The scripts will be execuated in numerical order so they can be numbered to meet dependancies. For example 00-bind-mount, 01-apache. Sample scripts are provided in sample.d.
