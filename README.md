## Alphafold configuration and tutorial IEO

#### installation of anaconda and configuration

Login onto the workstation, open your browser and go to

https://www.anaconda.com/

And download anaconda

Then, create a "software" directory in your user's space within data

    mkdir /data/username/software

Then, install anaconda there. From your home, go to the "Downloads" folder where the anaconda installer is located 

    cd ~/Downloads

And make anaconda executable

    chmod +x Anaconda3-2022.10-Linux-x86_64.sh

Then, run the installer

    ./Anaconda3-2022.10-Linux-x86_64.sh

CAREFUL!! DURING THE INSTALLATION, DO NOT INSTALL IN THE DEFAULT LOCATION by typing "yes"!! Install in the software directory by typing

    /data/username/software/anaconda3

as the install location. 

At the end of the install, agree to configure "conda init".

At the end of the installation, you can close and reopen the terminal. At this point, the word (base) should appear before your bash prompt.

Finish configuring by disabling autostart of anaconda.

    conda config --set auto_activate_base false

### Those that already have anaconda can start here

From this github repository, download the file containing the package list for the alphafold environment into your software directory

    cd /data/username/software/

    wget 

Then, create an environment called "alphafold" from the package list in the .yml file you just downloaded

    conda env create -f alphafold_env.yml


At this point, y