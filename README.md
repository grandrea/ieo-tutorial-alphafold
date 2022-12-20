## Alphafold configuration and tutorial IEO

### basic command cheat sheet

| command | Description               |
|---------|---------------------------|
| cd      | change directory          |
| pwd     | show me current directory |
| ls      | list files                |
| chmod   | change permissions        |
| mkdir   | create directory          |
| cp      | copy files                |
| rm      | delete files              |

options for each commmands are given as "flags" i.e. - signs after the command

    cp -r onedir another 

will copy a folder named "onedir" into a folder named "another" recursively (-r), meaning it will copy all the contents of onedir.

Places can be specified with paths or some shorthand locations

| location | meaning                       | example command |
|----------|-------------------------------|-----------------|
| ~        | home directory                | cd ~            |  
| ./       | current directory             | ls ./           |
| ../      | one directory up              | cd ../          | 
| /        | the top of the directory tree | ls /          | 

In general, just do [this tutorial](https://ubuntu.com/tutorials/command-line-for-beginners#4-creating-folders-and-files0), especially sections 4-6 . 

### installation of anaconda and configuration

Login onto the workstation, open your browser and go to

https://www.anaconda.com/

And download anaconda. Then open a terminal, which is where we will work the rest of the time. The terminal is located under "applications->system tools" or on your desktop.

Create a "software" directory in your user's space within data

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

If you do not see the word (base) before your prompt in the terminal, activate anaconda

    conda activate

Download the file containing the package list for the alphafold environment into your software directory

    cd /data/username/software/

    wget https://raw.githubusercontent.com/grandrea/ieo-tutorial-alphafold/main/alphafold_env.yml 

Then, create an environment called "alphafold" from the package list in the .yml file you just downloaded

    conda env create -f alphafold_env.yml


At this point, you should have an additional environment in addition to your base. check this by running

    conda info --envs

and you should see base and alphafold there.

### Optional: install the integrative modeling platformù

We will also work with the integrative modeling platform, IMP (www.integrativemodeling.org). This can be installed in a separate anaconda environment

    conda create -n imp python=3.9

then go to that environment

    conda activate imp

and install imp from the channel conda-forge (a community maintained set of tools that is similar to BioconductoR)

    conda install -c conda-forge imp

now you should have 3 anaconda environments: base, alphafold and imp. Any further work in integrative modeling will make use of imp.

### Running Alphafold

Alphafold is already configured on the workstation but you need some packages to run it, which is why we installed anaconda and installed some packages in a separate environment

Activate the alphafold environment

    conda activate alphafold

and navigate to the alphafold installation directory

    cd /data/software/alphafold

then you should be able to launch alphafold by typing

    python docker/run_docker.py --help
   
You should be greeted by a message like:

    Docker launch script for Alphafold docker image.
    flags:
    
    docker/run_docker.py:
      --[no]benchmark: Run multiple JAX model evaluations to obtain a timing that excludes the
        compilation time, which should be more indicative of the time required for inferencing
        many proteins.
        (default: 'false')
      --data_dir: Path to directory with supporting data: AlphaFold parameters and genetic and
        template databases. Set to the target of download_all_databases.sh.
      --db_preset: <full_dbs|reduced_dbs>: Choose preset MSA database configuration - smaller
        genetic database config (reduced_dbs) or full genetic database config (full_dbs)
        (default: 'full_dbs')
      --docker_image_name: Name of the AlphaFold Docker image.
        (default: 'alphafold')
      --docker_user: UID:GID with which to run the Docker container. The output directories will
        be owned by this user:group. By default, this is the current user. Valid options are:
        uid or uid:gid, non-numeric values are not recognised by Docker unless that user has
        been created within the container.
        (default: '1014:1014')
      --[no]enable_gpu_relax: Run relax on GPU if GPU is enabled.
        (default: 'true')
      --fasta_paths: Paths to FASTA files, each containing a prediction target that will be
        folded one after another. If a FASTA file contains multiple sequences, then it will be
        folded as a multimer. Paths should be separated by commas. All FASTA paths must have a
        unique basename as the basename is used to name the output directories for each
        prediction.
        (a comma separated list)
      --gpu_devices: Comma separated list of devices to pass to NVIDIA_VISIBLE_DEVICES.
        (default: 'all')
      --max_template_date: Maximum template release date to consider (ISO-8601 format: YYYY-MM-
        DD). Important if folding historical test sets.
      --model_preset: <monomer|monomer_casp14|monomer_ptm|multimer>: Choose preset model
        configuration - the monomer model, the monomer model with extra ensembling, monomer
        model with pTM head, or multimer model
        (default: 'monomer')
      --num_multimer_predictions_per_model: How many predictions (each with a different random
        seed) will be generated per model. E.g. if this is 2 and there are 5 models then there
        will be 10 predictions per input. Note: this FLAG only applies if model_preset=multimer
        (default: '5')
        (an integer)
      --output_dir: Path to a directory that will store the results.
        (default: '/tmp/alphafold')
      --[no]run_relax: Whether to run the final relaxation step on the predicted models. Turning
        relax off might result in predictions with distracting stereochemical violations but
        might help in case you are having issues with the relaxation stage.
        (default: 'true')
      --[no]use_gpu: Enable NVIDIA runtime to run with GPUs.
        (default: 'true')
      --[no]use_precomputed_msas: Whether to read MSAs that have been written to disk instead of
        running the MSA tools. The MSA files are looked up in the output directory, so it must
        stay the same between multiple runs that are to reuse the MSAs. WARNING: This will not
        check if the sequence, database or configuration have changed.
        (default: 'false')
    
    Try --helpfull to get a list of all flags.


We can then set up our first AlphaFold run.