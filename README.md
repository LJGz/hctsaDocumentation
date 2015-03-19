# Highly Comparative Time-Series Analysis: Manual

Guide to implementing the techniques and methods developed in [Highly Comparative Time-Series Analysis](http://rsif.royalsocietypublishing.org/content/10/83/20130048.full) using an interface between Matlab and a _mySQL_ server.

## Introduction
This document outlines steps required to set up and implement highly comparative analysis methods on a system using an interface between Matlab and a _mySQL_ database using the highly comparative time-series analysis repository.
The document is an accompaniment this code repository.

A summary of the steps involved in setting up the system is as follows, and will be elaborated on below:
1. Install and set up a mySQL server, setup a new database to store Matlab calculations in, and set up Matlab to be able to communicate with it, [here](setUpDatabase.md).
2. Populate the database with master operations, operations, and time series (using the **install.m** script followed by the user importing their own time series using **SQL_add**), [here](populateDatabase.md).
3. Run computations to evaluate the operations on all the time series using the **sample_runscript** as a template, [here](calculating.md).
4. Analyze the results of computations by retrieving calculated data using \textbf{TSQ\_prepared}, normalizing and filtering it using \textbf{TSQ\_normalize}, and then making use of a range of analysis and visualization scripts provided, Sec.~\ref{sec:analyzing}.