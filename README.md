<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
- [Installing R and R packages on Palmetto Cluster](#installing-r-and-r-packages-on-palmetto-cluster)
  - [R versions available on Palmetto Cluster](#r-versions-available-on-palmetto-cluster)
    - [The default (system) R](#the-default-system-r)
    - [R Modules](#r-from-modules)
    - [R from local installation](#r-from-local-installation)
    - [R from Python Anaconda](#r-from-python-anaconda)
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

### R From Modules

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

### R From Local Installation

It is also possible for users to build, configure, and install
customized version of R from source. To do so, the following
steps must be taken:

- Visit the R-project web page at https://cran.r-project.org/mirrors.html 
to select a mirror. In the next steps, it is assumed that the mirror from 
UC Berkeley is selected.

- Append */src/base/* to the mirror's URL (in this case
we have https://cran.cnr.berkeley.edu/src/base/)

- Select the major versions of R (R-0, R-1, R-2, R-3)

- Right click and copy the link to the specific version
of R source code that you would like to build (in this case we
choose 3.0.0

- Download the selected source code into a temporary directory 
on Palmetto

```bash
$ mkdir tmp

$ cd tmp

$ wget https://cran.cnr.berkeley.edu/src/base/R-3/R-3.0.0.tar.gz
--2016-10-10 14:49:49--  https://cran.cnr.berkeley.edu/src/base/R-3/R-3.0.0.tar.gz
Resolving cran.cnr.berkeley.edu... 169.229.201.201, 2607:f140:0:8000::201
Connecting to cran.cnr.berkeley.edu|169.229.201.201|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 25482059 (24M) [application/x-gzip]
Saving to: “R-3.0.0.tar.gz”

100%[===========================================================================================>] 25,482,059  9.28M/s   in 2.6s

2016-10-10 14:49:52 (9.28 MB/s) - “R-3.0.0.tar.gz” saved [25482059/25482059]

$ tar xzf R-3.0.0.tar.gz
```

- Assume that R is to be installed into /home/yourusername/software/, 
the following commands must be executed:

```bash
$ cd ~

$ mkdir software

$ cd tmp/R-3.0.0

$ ./configure --prefix=/home/yourusername/software

$ make

$ make install
```




### R From Python Anaconda

To create a robust environment that can support
multiple versions of R, the Palmetto 
cluster leverages Anaconda, 
Python's package and environment manager, to 
allow users to take control of the set up process. 

The only prerequisite for this approach is to 
load one of the Python Anaconda modules available on
Palmetto:

```bash
$ module avail anaconda
anaconda/1.9.1  anaconda/2.3.0  anaconda/2.4.0  anaconda/2.5.0  anaconda/4.0.0  anaconda3/2.5.0 anaconda3/4.0.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```


Currently, a conda environment containing R 3.3.1
is already set up for Anaconda3/2.5.0, and this is 
also the default R kernel for Palmetto's JupyterHub



```bash
$ module avail python

------------------------- /software/modulefiles -------------------------
python/2.7.6 python/3.3.3 python/3.4
```

Any of these modules can be loaded
to use a different version of Python than 2.6.6:

```bash
$ module add python/3.4

$ python --version
Python 3.4.2

$ which python
/software/python/3.4/bin/python
```

### Anaconda modules

There are also several "Anaconda" modules available on the cluster.

```bash
$ module avail anaconda
anaconda/1.9.1  anaconda/2.3.0  anaconda/2.4.0  anaconda/2.5.0  anaconda/4.0.0  anaconda3/2.5.0 anaconda3/4.0.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```

The `anaconda3` modules contain Python 3,
while the `anaconda` modules contain Python 2.
[Anaconda][anaconda-overview] is a Python "distribution",
which bundles together several packages
that are used in scientific computing and data analysis,
including `numpy`, `scipy`, `matplotlib`, `pandas`, and
several hundreds of others.
Using the Anaconda distribution removes the burden
of manually installing these packages from source.
Once the Anaconda module is loaded,
importing these packages "just works":

```bash
$ module add anaconda3/2.5.0
$ python
Python 3.5.2 |Anaconda 2.5.0 (64-bit)| (default, Jul  2 2016, 17:53:06)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tables       # imports the PyTables package
>>> tables.__file__     # prints where the tables module is loaded from
'/software/anaconda3/2.5.0/lib/python3.5/site-packages/tables/__init__.py'
```
Importantly, Anaconda provides the `conda` package manager,
which will be discussed later in this article.



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
2. Downloading and building the package and its dependencies yourself
3. Using the `conda`

Several packages will provide the option
to install in more than one way.
For example, see the [installation instructions
for the `mpi4py` package][mpi4py-install],
which can be installed either using `pip`,
or by downloading and building from source.

It is not necessary that you install *all* packages
using `pip`,
or build *all* packages yourself.
Your setup can contain several packages,
and each one may be installed in any of the above ways.

### Using `install.package` to install packages and dependencies from CRAN repository

`pip` is a program that installs Python packages,
and automatically installs any other Python packages
that are dependencies:

```bash
$ pip install package-name --user
```

The `--user` switch ensures that the package is installed
into your home directory, and not into the `/usr/local` directory.
This will only install the package
for the currently running version of Python.

Unfortunately,
`pip` is generally not well suited for installing scientific software packages
like `numpy` or `matplotlib` (see [here](https://packaging.python.org/science/) for explanation).

### Installing packages from file

When packages cannot be installed via `pip`
or when you want a package to be built in a specific way,
e.g., linked against specific libraries,
you may need to download and build the package for yourself.
The instructions for building a package are generally
a part of the package's documentation.
For example, see the instructions to manually install the `mpi4py` package
[here][mpi4py-install]. Importantly, notice the last step:

```bash
python setup.py install --user
```

The `--user` switch ensures that the package is installed
to your home directory, and not a directory like `/usr/local`.
Even when you manually build a package,
it is only compatible and available for the specific
version of Python loaded while building it.

When building packages yourself,
you will generally have to manage dependencies
yourself, i.e., they will not automatically be installed.

### Installing packages from development repositories

[mpi4py-install]: https://mpi4py.scipy.org/docs/usrman/install.html
[python-packaging-science]: https://packaging.python.org/science/
[anaconda-overview]: https://www.continuum.io/anaconda-overview
[anaconda-pkg-list]: https://docs.continuum.io/anaconda/pkg-docs
[conda-docs]: http://conda.pydata.org/docs/
