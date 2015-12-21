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

## 2.2. Configuration

## 2.3. Logging

## 2.4. Flags

## 2.5. Network

## 2.6. File and Folders

## 2.7. Shell and Processes

## 2.8. DataBase

## 2.9. Others

# 3. 3rd Party
