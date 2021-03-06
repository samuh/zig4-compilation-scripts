## Dynamic compilation of Zonation

These instruction describe the dynamic compilation of Zonation 4.0.0 on Ubuntu 14.04 (Trusty Tahr). Instructions have 
not been tested on other distributions/versions. You can copy the commands from this file or run the individual 
steps with the accompanied bash scripts. 

You execute all steps by running:

```
./00-run-all
```

### 1. Install Zonation dependencies

```
sudo apt-get update

sudo apt-get install cmake build-essential unzip libqt4-dev libfftw3-dev libqwt-dev libboost-all-dev libgdal-dev
``` 

Alternatively, run:

```
./01-deps-zig4
```

### 2. Get Zonation sources

Create a suitable download directory, fetch Zonation source code (version 4.0.0) from CBIG server, and extract the sources to the directory created:

```
mkdir zonation
wget https://github.com/cbig/zonation-core/archive/master.zip -P zonation
unzip zonation/master.zip -d zonation
```

Alternatively, run:

```
./02-wget-zig4
```

### 3. Build Zonation

To build Zonation library (`zig4lib`) and Zonation CLI utility (`zig4`), do the following:

```
mkdir zonation/build
cd zonation/build
cmake ../zonation-core-master
make
```

If you have several cores available for compilation, you can pass switch `-j X` to `make` where `X` is the number of designated cores (e.g. `make -j4`).

Alternatively, run:

```
./03-build-zig4
```

### 4. Make Zonation available system wide (optional)

In order to call `zig4` anywhere on the system (instead of just the build-location), create a symbolic link:

```
sudo ln -s FULL_PATH/zonation/build/zig4/zig4 /usr/local/bin/zig4
```

Replace `FULL_PATH` with the full path to the directory containing directory `zonation` created in step 2.

Alternatively, run:

```
./04-postinstall-zig4
```

## Testing Zonation using the tutorial data

This stage is completely optional.

You can test that Zonation works by running some of the runs used in the [Hunter Valley tutorial](https://github.com/cbig/zonation-tutorial). This is not a thorough test, but it will at least give you an overview on whether Zonationis working as intended. Tutorial data and setup files are fetched using [git](http://git-scm.com/) and run using [ztools](https://github.com/cbig/ztools).

### 5. Install ztools dependencies

First, install git:

```
sudo apt-get -y install git
```

Then, install Python packages needed by ztools:

```
sudo apt-get -y install python-yaml python-pip 
```

Alternatively, run:

```
./05-deps-ztools
```

### 6. Install ztools

`ztools` is installed directly from GitHub using [pip](http://www.pip-installer.org/en/latest/).

```
sudo pip install https://github.com/cbig/ztools/archive/master.zip
```

Alternatively, run:

```
./06-install-ztools
```

### 7. Clone Zonation tutorial using git

With git installed, clone the tutorial repository with the following command:

```
git clone https://github.com/cbig/zonation-tutorial.git
``` 

Alternatively, run:

```
./07-git-tutorial
```

### 8. Run the tutorial runs

You can run [5 basic tutorial variants](https://github.com/cbig/zonation-tutorial/tree/master/basic) defined in the configuration file `tutorial_runs.yaml` by using `zrunner` utility in ztools:

```
zrunner -l tutorial_runs.yaml
```

zrunner will produce an output file `results_XXX.yaml` in the same folder. `XXX` will correspond to information about your system. If everything went fine, you should see no critical errors on the screen and the yaml-file should report execution times for successful runs.
