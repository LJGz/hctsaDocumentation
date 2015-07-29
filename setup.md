# Getting started

The *hctsa* package can either be used in two ways:

1. Completely within Matlab, for quick and easy analysis of a particular time-series dataset
2. Using a link between Matlab and a *mySQL* database, which allows the user to accommodate datasets that may grow over time, or to exploit distributed computation (across nodes of a compute cluster, for example).

## 1. Installing the *hctsa* package **without** a link to a *mySQL* database
This is the simplest way to get the *hctsa* working on a time-series dataset.
The *hctsa* package can be installed by running the `install` script, which adds the required paths to dependent toolboxes, and compiles mex binaries to work on your system.
This is a one-off installation step; future work can involve simply loading the paths (using the `startup` script).

### 


## 2. Installing the *hctsa* code package to work with a *mySQL* database

The *hctsa* package requires some preliminary set up to work with a *mySQL* database, described [here](mysql_database.md):

1. Installation of *mySQL*, either locally, or on an accessible server.
2. Setting up Matlab with a *mySQL* java connector (done by running the `install_jconnector` script in the **Database** directory, and then restarting Matlab).

After the database is set up, the rest of the package can be installed by running the `install` script, which:

1. (if required) Sets up Matlab to be able to communicate with the *mySQL* server and creates a new database to store Matlab calculations in, described [here](mysql_database.md).
2. Populates the database with our default library of master operations and operations, as described [here](populating.md). (NB: a description of the terminology of 'master operations': a set of input arguments to an analysis function, and 'operations': a single time-series feature, is [here](populating.md)).
3. Compiles **mex** binaries required to evaluate all operations, described [here](compiling_binaries.md). In addition to the mex files compiled by the `install` script, the user is additionally required to compile the *TISEAN* binaries if desired [in the commandline](compiling_binaries.md).


This section contains additional details about each of these steps.

Note that the above steps are one-off installation steps; once the software is installed and compiled, a typical workflow will simply involve opening Matlab, running the `startup` script (which adds all paths required for the *hctsa* software), and then working within Matlab from any desired directory.

### Importing a time-series dataset

Once installed, if using our default library of operations, the typical next step is to [add a dataset of time series](adding_time_series.md) using the `SQL_add` command.
Custom [master operations](adding_master_operations.md) and [operations](adding_operations.md) can also be added, if required.

### Computation, processing, and analysis

After installing the software and importing a time-series dataset to a *mySQL* database, the process by which data is retrieved from the database to local Matlab files (using `SQL_retrieve`), feature sets computed within Matlab (using `TS_compute`), and computed data stored back in the database (`SQL_store`) is described in detail [here](calculating.md).

After the computation is complete for a time-series dataset, a range of processing, analysis, and plotting functions are also provided with the software, as described [here](analyzing_visualizing.md).


<!--1. Sets up a *mySQL* server and database, populates the database with our standard library of functions and operations, and then compiles all of the mex functions required by Matlab to run all of the operations.-->

<!--## Setting up-->
<!--{#sec:SettingUp}-->

<!--This section describes initial tasks that one must perform once, to set up the *mySQL* database and its interface with Matlab.-->


<!--### Setting the path-->
<!-- {#sec:settingPath} -->
