This document is to guide researchers of the Klotsa Group on how to set up a research environment on different machines to run molecular dynamics simulations using [HOOMD-blue](http://glotzerlab.engin.umich.edu/hoomd-blue/) Molecular Dynamics simulation. Just to clarify, the purpose of this document was to provide something that anyone with any background can just follow and have the setup working, and thus the methods provided here may not be the most efficient or elegant ways of setting these up.

# System Setup Guides

- [1. Introduction](#1-introduction)
- [2. Setting up the environment](#2-setting-up-the-environment)
  - [2-1. Macintosh](#2-1-macintosh)
  - [2-2. Windows](#2-2-windows)
  - [2-3. Longleaf Cluster](#2-3-longleaf-cluster)
- [3. Setting up Miniconda](#3-setting-up-miniconda)
- [4. Installing prerequisite/post-processing softwares](#4-installing-prerequisitepost-processing-softwares)
- [5. Building HOOMD-blue from source](#5-building-hoomd-blue-from-source)
- [6. Troubleshooting](#6-troubleshooting)

## 1. Introduction

This guide will walk you through the installation process for HOOMD-blue (<http://glotzerlab.engin.umich.edu/hoomd-blue/>) molecular dynamics simulation package and some commonly used data processing tools on different types of machines.

## 2. Setting up the environment

This section will go over setting up the environment on personal machines such as a [Macintosh](#2-1-macintosh) or a [Windows PC](#2-2-windows) as well as setting up the environment on UNC's [Longleaf Cluster](#2-3-longleaf-cluster). For Linux users, skip to section 3, [Setting up Miniconda](#3-setting-up-Miniconda).

### 2-1. Macintosh

Note that the most current software does not always mean they are the most stable versions. As a reference, the following installation guide was carried out on
>macOS Big Sur, Version 11.6.5

Also, the following installation guide assumes your machine has x86_64 (AMD64) architecture instead of the ARM M label chips used for recent Macs. Given Apple's Rosetta 2, M label chips should not prohibit you from following this guide, but know that there is likely to be a drop in the performance of your HOOMD simulations ran locally. Making sure you are following this guide but switch all the x86_64 specific instructions to corresponding ones for ARM may prevent this issue depending on the software support for ARM architecture. As I have no experience working with the latest Apple machines, I cannot provide further assistence in this guide should there be any problem.

Homebrew (<https://brew.sh/>) is a package manager that will help you to install and manage the required software packages easily. If you're familiar with Linux systems, think of Homebrew as apt-get on Debian-based systems or dnf/yum on RedHat-based systems.

Go to <https://brew.sh> and run the installation command shown in Terminal.

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This will install Homebrew package manager as well as the Command Line Tools that are used to carry out the following guide.

Git will be installed with the following commands.

```zsh
brew install git
```

Note that macOS's default shell has changed to zsh from bash. There should not be much significant differences but if you do prefer to use bash, install bash via homebrew `brew install bash` and then changing the default shell with `chsh -s /usr/local/bin/bash`.

Your system is now set up to install Miniconda and proceed with the installation of HOOMD-blue.

### 2-2. Windows

Unfortunately, Windows is not the optimal system to set up HOOMD-blue molecular dynamics package as it isn't supported out of the box. **However** it is still possible to use Windows machines to run HOOMD-blue simulations. To set up a Windows system to install and run the HOOMD-blue simulation package, you will need to have Windows Subsystem for Linux (WSL) set up.

Follow the guide from Microsoft ([https://learn.microsoft.com/en-us/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install)).

On Windows, I highly recommend using the Windows Terminal ([https://github.com/microsoft/terminal](https://github.com/microsoft/terminal)) and VS Code ([https://code.visualstudio.com/](https://code.visualstudio.com/)) as they work well with WSL out of the box. If you choose to use VSCodium ([https://vscodium.com/](https://vscodium.com/)) instead of VS Code, you may have some problems connecting to WSL easily.

### 2-3. Longleaf Cluster

To set up the environment on Longleaf Cluster ([https://help.rc.unc.edu/longleaf-cluster/](https://help.rc.unc.edu/longleaf-cluster/)), I highly suggest you to check out [this](../misc_guides/setting_up_supercomputer_account.md) document before proceeding with this guide further to set up your Longleaf account.

You just need to load some modules to be used for setting up the environment and GPU usage.
```bash
module load git cuda
module save
```

## 3. Setting up Miniconda

On Linux, Macintosh, and the cluster, these will be done on your terminal. On Windows, make sure you're on your WSL terminal. If you prefer, you could use system Python on your personal machines, but using Miniconda (Anaconda) ([https://www.anaconda.com/](https://www.anaconda.com/)) will make your life infinitely easier if you have to modify anything about your environment.

For **Macintosh**,
```bash
mkdir -p ~/miniconda3
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh -o ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
$HOME/miniconda3/bin/conda init bash zsh
source $HOME/.bashrc
```

For everything else,
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
$HOME/miniconda3/bin/conda init bash
source $HOME/.bashrc
```

Then create your environment 
```bash
conda create -y -n hoomd
```

to install Python and everything else. You can name the environment differently if you would prefer it to be something else. Just remember and modify the commands below accordingly.

## 4. Installing prerequisite/post-processing softwares

First, activate your environment with 
```bash
conda activate hoomd
```

Then, the prerequisite software packages can be installed with the following.
```bash
mamba install -y --strict-channel-priority -c https://conda.ovito.org -c conda-forge c-compiler cxx-compiler git make cmake pybind11 eigen tbb tbb-devel python numpy matplotlib ovito freud gsd xvfbwrapper
```

Now the prerequisite and some post processing softwares are installed and the environment is ready to compile HOOMD-blue.

## 5. Building HOOMD-blue from source

Most of the instructions below are from the official documentation ([https://hoomd-blue.readthedocs.io/en/stable/building.html](https://hoomd-blue.readthedocs.io/en/stable/building.html)).

Start with obtaining the source.
```bash
git clone --recursive https://github.com/glotzerlab/hoomd-blue
```

Configure the build with:

On the cluster:
```bash
CC=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-cc CXX=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-c++ cmake -B build/hoomd -S hoomd-blue -DCMAKE_INSTALL_PREFIX=`python3 -c "import site; print(site.getsitepackages()[0])"` -DCMAKE_CXX_FLAGS=-march=native -DCMAKE_C_FLAGS=-march=native -DENABLE_GPU=ON -DENABLE_TBB=ON
```

On personal machines:
```bash
CC=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-cc CXX=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-c++ cmake -B build/hoomd -S hoomd-blue -DCMAKE_INSTALL_PREFIX=`python3 -c "import site; print(site.getsitepackages()[0])"` -DCMAKE_CXX_FLAGS=-march=native -DCMAKE_C_FLAGS=-march=native -DENABLE_TBB=ON
```

If there was no failure, build the package.
```bash
cmake --build build/hoomd -j8
```

Once the build is complete, install the package.
```bash
cmake --install build/hoomd
```

Now your HOOMD-blue and the installed post-processing tools are ready to be used.

## 6. Troubleshooting
TO BE UPDATED