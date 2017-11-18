<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
- [Installing R and R packages on Palmetto Cluster](#installing-r-and-r-packages-on-palmetto-cluster)
  - [R versions available on Palmetto Cluster](#r-versions-available-on-palmetto-cluster)
    - [The default (system) R](#the-default-system-r)
    - [R Modules](#r-from-modules)
    - [R from local installation](#r-from-local-installation)
    - [R and RStudio from Python Anaconda](#r-and-rstudio-from-python-anaconda)
    - [Which R should I use?](#which-r-should-i-use)
  - [Installing R packages](#installing-r-packages)
    - [Using `install.package` to install packages and dependencies from CRAN repository](#using-install.packages-to-install-packages-and-dependencies)
    - [Installing packages from file](#installing-packages-from-file)
    - [Installing packages from development repositories](#installing-packages-from-development-repositories)
<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Installing R and R packages on Palmetto Cluster

This article aims to provide detail instructions on how to 
set up R and subsequently install R community packages on the Palmetto
cluster. 

The core R functionalities provide users with an easy-to-use framework to install 
community packages. The default configuration of this framework requires 
administrative priviledge, which is not available for user accounts on Palmetto. 

This article will explain how users 
can correctly set up R and install packages on Palmetto within the 
limitation of user accounts. 

In particular, this article will cover:

* All the available versions of R on the cluster
* The different ways for users to install R packages

## R versions available on Palmetto Cluster

Unlike Python, R does not have different major versions being
supported at the same time (e.g., Python 2.x and 3.x). However, 
there are community packages that are not updated as R moves forward, 
and there exists the need for users to set up their own versions 
of R. 

### The default (system) Python

To identify the version of the current R software, you can run R with the *--version*
flag. The version of the default R software on Palmetto is:

```bash
$ module list
No Modulefiles Currently Loaded.

$ R --version
R version 2.13.0 (2011-04-13)
Copyright (C) 2011 The R Foundation for Statistical Computing
ISBN 3-900051-07-0
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License version 2.
For more information about these matters see
http://www.gnu.org/licenses/.
```


The "default" version of R on the Palmetto cluster
is **2.13.0**, released in 2011. R has undergone 
significant updates since then, and in general,
it is highly recommended that users **not** rely 
on this version of R to run their own applications:

1. Newer libraries will require other versions
of R, especially with R's transition to major 
revision 3.0 in 2013 to support 64-bit calculations.

2. The system version of R may change.
Any applications or packages that users
may build using the system R
will likely not run if this happens.

### R from Modules

R is also available on Palmetto under the form of 
software modules. These are copies of different versions
of R that are installed and configured for the cluster and
can be activate via Palmetto's module capability. 

```bash
$ module avail R
R/3.0.2 R/3.2.2

$ module load R/3.0.2
$ R --version
R version 3.0.2 (2013-09-25) -- "Frisbee Sailing"
Copyright (C) 2013 The R Foundation for Statistical Computing
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
http://www.gnu.org/licenses/.
```

While it is possile to use the R modules on Palmetto, 
the versions of R listed here are not up to date, and the rate of
new releases from R (current version 3.3.1) makes it 
difficult to maintain numerous versions
of R in a shared computing environment like Palmetto.

### R from Local Installation

It is also possible for users to build, configure, and install
customized version of R from source. To do so, the following
steps must be taken:

- Create a temporary installation directory
```bash
$ cd $HOME
$ mkdir tmp
$ mkdir -p software/packages
```

- Dependency Installation: 

```bash
$ cd $HOME/tmp
$ wget http://zlib.net/zlib-1.2.11.tar.gz
$ wget http://www.bzip.org/1.0.6/bzip2-1.0.6.tar.gz
$ wget http://tukaani.org/xz/xz-5.2.2.tar.gz
$ wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.40.tar.gz
$ wget https://www.openssl.org/source/old/1.0.0/openssl-1.0.0k.tar.gz
$ wget --no-check-certificate https://curl.haxx.se/download/curl-7.47.1.tar.gz
$ tar xzf zlib-1.2.11.tar.gz
$ tar xzf bzip2-1.0.6.tar.gz
$ tar xzf xz-5.2.2.tar.gz
$ tar xzf pcre-8.40.tar.gz
$ tar xzf openssl-1.0.0k.tar.gz
$ tar xzf curl-7.47.1.tar.gz
$ cd zlib-1.2.11
$ ./configure --prefix=$HOME/software/packages
$ make
$ make install
$ cd ../bzip2-1.0.6
$ make -f Makefile-libbz2_so
$ make clean
$ make
$ make -n install PREFIX=$HOME/software/packages
$ make install PREFIX=$HOME/software/packages
$ cd ../xz-5.2.2
$ ./configure --prefix=$HOME/software/packages
$ make -j3
$ make install
$ cd ../pcre-8.40
$ ./configure --enable-utf8 --prefix=$HOME/software/packages
$ make -j3
$ make install
$ cd ../openssl-1.0.0k
$ ./config --prefix-$HOME/software/packages --openssldir=$HOME/software/openssl shared
$ make
$ make install
$ cd ../curl-7.47.1.tar.gz
$ ./configure ./configure --prefix=$HOME/software/packages --with-ssl=$HOME/software/openssl
$ make
$ make install
$ export PATH=$HOME/software/packages/bin:$PATH
$ export LD_LIBRARY_PATH=$HOME/software/packages/lib:$LD_LIBRARY_PATH 
$ export CFLAGS="-I$HOME/software/packages/include" 
$ export LDFLAGS="-L$HOME/software/packages/lib"
```


- Download the selected R source code into a temporary location in your home directory 
on Palmetto and install R into /home/yourusername/software/R/3.4.0

```bash
$ cd $HOME/tmp
$ wget https://cran.r-project.org/src/base/R-3/R-3.4.0.tar.gz
$ tar xzf R-3.4.0.tar.gz
$ cd tmp/R-3.4.0
$ ./configure --prefix=/home/yourusername/software/R-3.4.0
$ make
$ make install
```

- To use this location installation of R, path to its 
executables must be exported:

```bash
$ export R_HOME=/home/yourusername/software/R-3.4.0
$ export PATH=$R_HOME/bin:$PATH
$ R                                                                    
                                                                                                
R version 3.4.0 (2017-04-21) -- "You Stupid Darkness"                                           
Copyright (C) 2017 The R Foundation for Statistical Computing                                   
Platform: x86_64-pc-linux-gnu (64-bit)                                                          
                                                                                                
R is free software and comes with ABSOLUTELY NO WARRANTY.                                       
You are welcome to redistribute it under certain conditions.                                    
Type 'license()' or 'licence()' for distribution details.                                       
                                                                                                
  Natural language support but running in an English locale                                     
                                                                                                
R is a collaborative project with many contributors.                                            
Type 'contributors()' for more information and                                                  
'citation()' on how to cite R or R packages in publications.                                    
                                                                                                
Type 'demo()' for some demos, 'help()' for on-line help, or                                     
'help.start()' for an HTML browser interface to help.                                           
Type 'q()' to quit R.      
```


### R and RStudio from Python Anaconda

To create a robust environment that can support
multiple versions of R, we recommend user to leverage Anaconda, 
Python's package and environment manager, to 
allow users to take control of the set up process. 

There exists a number of Python Anaconda modules on
Palmetto:

```bash
$ module avail anaconda
anaconda/2.5.0          anaconda/4.3.0(default) anaconda3/4.2.0
anaconda/4.2.0          anaconda3/2.5.0         anaconda3/4.3.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```
The highest possible version of R supported by Palmetto's Anaconda is R 3.4.1 (anaconda3/4.3.0). Version 4.2.0 of Anaconda 
(anaconda and anaconda3) also supports R 3.4.1; however, trying to set up R in this conda is not straight forward since an 
existing version of R (3.3.1) was already installed to support the R kernel of JupyterHub. 

To set up R 3.4.1 in anaconda3/4.3.0, the following steps must be done:

```
$ module load anaconda3/4.3.0
$ conda create --name rstudio-anaconda3-4.3.0 python=3.6
$ source activate rstudio-anaconda3-4.3.0
$ conda config --add channels conda-forge
$ conda config --add channels r
$ conda install r
$ conda clean --all
```
If you want to have a later version of R (3.4.2) and also to install RStudio (1.1.383), one of the most popular IDE for R, 
you will need to install a local version of Anaconda in your home directory. 

```
$cd tmp 
$ wget https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
$ sh Anaconda3-5.0.1-Linux-x86_64.sh
```

*The download link of `wget` can change over time. You can visit [Continuum's website](https://www.anaconda.com/download/#linux) 
to search for the latest version of Anaconda*. 

Assuming that you choose to install anaconda into a directory called **software** in 
your home directory, you can follow the next steps to set up R and RStudio:

```
$ export PATH=/home/$USER/software/anaconda3/bin:$PATH
$ conda create --name rstudio-anaconda3-5.0.1 python=3.6
$ source activate rstudio-anaconda3-5.0.1 python=3.6
$ conda config --add channels conda-forge
$ conda config --add channels r
$ conda install r
$ conda clean --all
$ conda install rstudio
$ conda clean --all
```

To run rstudio, you will need to follow the 
[Palmetto User Guide](https://www.palmetto.clemson.edu/palmetto/userguide_howto_run_graphical_applications.html) 
to run graphical applications on Palmetto. After finished requesting a compute node with graphical application support, 
you can run the following:

```
$ export PATH=/home/$USER/software/anaconda3/bin:$PATH
$ source activate rstudio-anaconda3-5.0.1
$ rstudio
```


### Which R should I use?

In general,
the Python versions provided by the `anaconda` modules
may be a good starting point for most users,
as they also provide several widely-used packages.
However, if you want to build and maintain your own versions
of Python packages,
you may want to use the `python` modules.

Later in this article,
we will also see another option:
using `conda` to easily install *any* version of Python,
and easily install packages for this version.

## Installing R packages

Although the `anaconda` modules already provide several of the
widely used packages,
you may need to install other packages or different versions
of the available packages for your own project.
This can sometimes be a challenge,
because some packages may require other packages (i.e., they have dependencies),
and because users do not have root (administrative) privileges on the cluster.
Most packages and their dependencies can be installed in one of the following ways:

1. Using `install.packages`
2. Installing packages from files
3. Installing packages from development repositories outside of CRAN. 

### Using `install.packages` to install packages and dependencies from CRAN repository

When you first call `install.packages` inside R, a prompt will pop up saying that you do not have permission to 
install into the root directory path, and R will offer to install packages into a local directory inside your 
home directory. Go ahead and accept this offer. From this point on, all packages will be installed into 
this local directory and R will automatically remember this location for future references. 

### Installing packages from file

You can download the packages directly from CRAN and use `R CMD INSTALL <package file>` to install these packages from 
the Linux terminal on Palmetto. 

### Installing packages from development repositories

There are many useful R packages that are not part of the CRAN repository. For those that are available on Github, the `devtools` 
library can be used to install them. 

If you use R installed through `conda`, it is possible to also install packages for R from the `conda` world. In many cases, these 
present significant advantages over the traditional methods (install.packages() or `devtools`), as `conda` will take care of any 
non-R dependencies that need to be installed. 
