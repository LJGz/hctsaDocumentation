# Computing operations and writing back to the database

After retrieving data from the *mySQL* database, missing entries (NULL in the database -> NaN in the local Matlab file)

## Performing calculations using `TSQ_brawn`
<!--{#sec:performing_calculations}-->

Once a relevant section of the data matrix has been retrieved, we can calculate values that have not previously been calculated, indicated by NULL entries in the **Results** table of the database, and NaN entries in the corresponding `HCTSA_loc.mat`.

Calculations are performed using the function `TSQ_brawn`, which stores results back into the matrices in `HCTSA_loc`. This function is generally run without inputs:

        TSQ_brawn;

Inputs to the function are optional and can be used to specify whether to log to file or the screen (‘tolog’, default: no), and whether to compute operations across cores using the Parallel processing (‘toparallel’, default: no). Running `TSQ_brawn` will begin running operations on time series in `HCTSA_loc.mat` for which elements in **TS\_DataMat** are NaNs (indicating that they have not been run
before), and storing the results back in the matrices of `HCTSA_loc.mat`, i.e., **TS\_DataMat** (output of each operation on each time series), **TS\_CalcTime** (calculation time for each operation on each time series), and **TS\_Quality** (labels indicating errors or special-valued outputs). When all NULL entries in **TS\_DataMat** have been calculated, **TSQ_brawn** saves the results back to the local file: `HCTSA_loc.mat`.
These results can then be written back to the database using `TSQ_agglomerate`, as shown in Fig.[fig:ComputationSchematic].

## Writing calculations back to the database using `TSQ_agglomerate`
<!--{#sec:writingCalcsDatabase}-->

Once calculations have been performed using Matlab on local files, the results must be written back to the database.
This task is performed by `TSQ_agglomerate`, which reads the data in `HCTSA_loc.mat`, checks that the metadata still matches the database, and then begins updating the **Output**, **Quality**, and **CalculationTime** columns of the **Results** table in the *mySQL* database.
This can be done by simply running:

        TSQ_agglomerate;
