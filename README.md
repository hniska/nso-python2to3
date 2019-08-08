# Description
Example on how to run both python2 and python3 packages simultaneously in NSO.

It works by changing the python VM startup script so that for each python package that starts it will check if the package name is in a file (python3packages.txt), if it is it will start that package with python v3. All the other packages will be started with python v2.

Small headsup, this script is not a supported solution :)

## Installation

If you are running a local installation (i.e not a system install) you just have to create a bin folder in you NSO runtime folder and copy the two files there
```
cd your_nso_runtime/
mkdir bin
cp path_to/nso-python2to3/ncs-start-python-vm bin/
touch bin/python3packages.txt
```
then change your ncs.conf so that it executes your new ncs-start-python-vm script intead of the default one.
```
vi ncs.conf
....
   <python-vm>
       <start-command>bin/ncs-start-python-vm</start-command>
   </python-vm>
....

```

Then just add the packages you want to be running with python3 to the python3packages.txt file

```
cat python3packages.txt
external-id-allocator
selftest
l3vpn
```

Restart NSO, the restart is only needed for NSO to read in the new python vm startup script, might even be enough with a ncs --reload. Adding new packages to python3packages.txt will not require a reload.
```
ncs --stop
ncs
```

## Contact

Contact Hakan Niska <hniska@cisco.com> with any suggestions or comments. If you find any bugs please fix them and send me a pull request.
