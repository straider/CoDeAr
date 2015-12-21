# CoDeAr
========

Command Line re-usable scaffold for coding small units of work that follow the Collect-Deliver-Archive "rhythm".

# 1. Overview

Quite often it is necessary to code processes that run on command-line mode ( CLI ). Usually these processes require a common set of procedures or steps, in any technology, before actually coding their own specific logic and features. This preparation work often deals with command-line options handling, configuration files and logging.

What if there would be no need for this preparation work?

What if with minimal to no effort it would be possible to just focus on implementing each process specific logic and features?

## 1.1. Goal and Objectives

- Minimize the time and effort required to implement a process from scratch;
- Provide, at the very least, a set of templates in each technology with frequently used features;
- Provide a re-usable scaffold that supports frequently used features;
- Implement this simple command-line kit in distinct technologies:
  - Shell
  - Perl
  - Python
  - Ruby

**Note**: For each technology there should exist a file that registers similar work.

## 1.2. Scope

Although this re-usable scaffold can be seen as a library or framework it should be considered as a set of best-practices to follow.

Currently there are no plans to make this a tool.

## 1.3. Terms and Concepts

Before using the code one should get acquainted with these terms and concepts:

- **worker**: a process, built around this "_scaffold_", that executes one and one task only. The task is composed of sequential steps:
  - **collect**: step that collects data, usually from files or from tables;
  - **process**: specific step of each work that processes the data after being collected and before being delivered;
  - **deliver**: step that gathers the result from the process step and publishes them as deliverables of the worker, usually to files or to tables;
  - **archive**: final step that archives the deliverables of the process, optionally by compressing them in one of the well-known formats ( such as gz or bz2 ).
-  **orchestrator**: a wrapper of workers that passes the output of a worker as input to another worker. This special process chains together a set of workers in order to accomplish a complex task ( or multiple tasks ).

# 2. Milestones

These milestones, whenever feasible, should be accomplished with core and standard library features. Although 3rd party libraries are allowed, their usage should be limited and avoided as much as possible.

## 2.1. Command-Line Options

Every worker has to obbey to a well-known set of command-line options:

- **-h / --help**: shows the usage message for the worker;
- **-v / --version**: shows the worker's name and version;
- **-c / --configuration-file**: loads a specific flat configuration file, bypassing the default configuration;
- **-s / --show-configuration**: outputs the complete configuration, in a given format ( yaml, json );
- **-g / --get-folder**: shows the absolute path of the given folder name ( input, working, output, archive ).

**Note**: specific command-line options are only known ( and handled ) by the worker and should not be "configurable".

## 2.2. Configuration

The configuration files can be in any of the following formats:

- YAML
- JSON
- XML
- INI

**Note**: All configuration files are located in a folder relative to the top folder of the worker.

The configuration works under one of two modes:

- **Multiple**: each configuration file is merged against the current configuration in order to fullfil a complete configuration;
- **Flat**: only one configuration file is read and it must correspond to a complete configuration.

The default mode is to read multiple configuration files and merged them into one virtual complete file. The flat mode must be enabled through a specific command-line option.

**Note**: one improvement could be to enable to disable the default configuration file, in any of the two modes.

The features provided by this layer are:

- **Load Configuration Files**: loads multiple configuration files, in a specific order of formats and where all file have the same name of the worker;
- **Load Configuration File**: loads a specific complete configuration file, in any of the allowed formats;
- **Show Configuration**: outputs the complete configuration, in one of the allowed formats ( specified as argument );
- **Get Configuration**: this is actually a set of sub-features that allows access to frequently used sections of the complete configuration:
  - **Logging**;
  - **Folders**;
  - **Flags**;
  - **DataBases**;
  - **Worker**.
- **Expand Properties**: in order to allow configuration files that hold values that are dependant on other properties it is necessary to control when these properties must be expanded on those values;
- **Add Property**: sometimes it's necessary to control other properties that are not configurable but that are useful when expanding properties.

## 2.3. Logging

Logging messages should be outputted to the console only and not to a file when there's no configuration. But by default the logging messages should be outputted to the console and to a file, both set at INFO logging level.

The pattern for the logging messages should follow the formatting placeholders of Log4J. At the bare minimal there should be 3 fields at least: timestamp, logging level and logging message. But if feasible then at least 2 more fields should be presented: filename and line number.

The features provided by this layer are:

- **Create Logger**: each worker must have access to a logger instance;
- **Reconfigure Logger**: in order to deal with multiple configuration files it is necessary to allow the logging mechanism to be reconfigured when a complete configuration is loaded;
- **Shutdown Logger**: it's useful for cases where the logging file should be compressed.

**Note**: the logging messages must be encoded in UTF-8.

## 2.4. Flags

The features provided by this layer are:

- **Initialize**: initializes the flag mechanism, if explicitly defined in configuration section;
- **Set _Processing_**: sets the flag as _worker still executing_, which should prevent other instances of that worker to run simultanenously;
- **Set _Terminated_**: sets the flag as _worker terminated successfully_, which should enable another instance of that worker to run;
- **Set _Errored_**: sets the flag as _worker terminated abnormally_, which should prevent other instances of that worker to run simultanenously, until the root cause of the problem is fixed.

## 2.5. Network

The features provided by this layer are:

- **Open Connection**: establishes a connection with the configured remote server;
- **List Files**: returns a list of files in current remote folder;
- **Get File**: collects a remote file given it's filename;
- **Put File**: delivers a local file to a remote location.

The protocol used is one of the following well-known file transfer protocols:

- **FTP**
- **SFTP**

## 2.6. File and Folders

The features provided by this layer are:

- **Create Missing Folders**: creates missing folders, suited for all the well known folders, which when do not exist are then created automatically;
- **Create File**: operating system agnostic way of creating an empty file;
- **Create Folder**: operating system agnostic way of creating an empty folder;
- **Remove**: operating system agnostic way of deleting a file;
- **Copy**: operating system agnostic way of safely copying a file, in 2 stages: first copies the file as a temporary file and if successfully then rename it to the final file;
- **Move**: operating system agnostic way of safely moving a file, in 3 stages: first copies the file as a temporary file, if successfully then rename it to the final file and then removes the original file;
- **Collect**: moves an input file from the input folder into the working folder, by using the safe move feature;
- **Deliver**: moves an ouput file from the working folder into the output folder, by using the safe copy feature;
- **Clean**: removes the output files that are not deliverables of the worker;
- **Archive**: archives the final files, optionally compressing them by using one of the well known formats: gz, bz2, zip.

**Note**: the deliver feature must copy the final file, instead of moving it, because the archive feature may require this final file at the working folder. If the archive feature uses the final file on the output folder then it will only work when there's an output folder configured, even in cases where the deliverable produced is not to be stored in the output folder. But in which real-case scenario is this useful?

## 2.7. Shell and Processes

The features provided by this layer are:

- **Execute Command**: executes another process as a _child_ of the worker.

## 2.8. DataBase

The drivers, for each database engine, should be loaded dynamically. This means that only the drivers explicitly set on the configuration are the drivers that should be loaded.

The features provided by this layer are:

- **Open Connection**: establishes a connection with the configured database engine;
- **Create Cursor**: opens a cursor, given a connection;
- **Get Table Columns**: returns meta information about a table, at least the names of columns if nothing more;
- **Table Exists?**: checks if a table exists;
- **Execute Query**: executes a SELECT statement and returns all rows;
- **Execute SQL**: executes a SQL statement, DML or DDL, and returns the number of affected rows;
- **Import Records**: given a table and a set of working files then load each line as a record on that table;
- **Extract Records**: given a SELECT statement and a filename then save every record from that query as a line on the file;
- **Update Records**: given a function to transform a line into a criteria it will update every record of a table that matches that criteria.

**Note**: since not all workers require database connections then neither a connection must be established and neither any driver must be loaded for the code to work.

## 2.9. Others
