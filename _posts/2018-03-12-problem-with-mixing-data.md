---
layout: post
title: The problem with mixing data
category: resources
image: /assets/images/mixing_sugar_and_cinnamon.jpg
---

You are running out of space on your computer and you need to delete files from
old projects.  If this concept fills you with fear of boredom, or worse, fear
of loosing data then read on. Here are some practical advice on how to
structure files and directories to make this easier in the future.

### The problem

You have run out of disk space. You need to delete unnecessary files from an
old project. Below is a tree view of the files in the old project.

```
$ tree some_project/
some_project/
├── analysis.py
├── data.csv
├── more_data.csv
├── multiple_file_analysis.py
├── output.csv
├── pre_process.py
└── cleaned_data.csv
```

*What files can you safely delete?*


### What a mess

This toy example may seem artificial, but it is based on a working pattern that
is very common.

When one starts working on a project it is convenient to keep scripts close
to the raw data. A project may therefore start out along the lines of the below,
with a data file (``data.csv``) and an analysis script (``analysis.py``).


```
$ tree some_project/
some_project/
├── analysis.py
└── data.csv
```

The analysis script works on the data to produce an output file (``output.csv``).

```
$ tree some_project/
some_project/
├── analysis.py
├── data.csv
└── output.csv
```

As it turns out the data needs some pre-processing to standardise it.
One therefore creates a script to do this pre-processing (``pre_process.py``)
which generates cleaned data (``cleaned_data.csv``).

```
$ tree some_project/
some_project/
├── analysis.py
├── data.csv
├── output.csv
├── pre_process.py
└── cleaned_data.csv
```

Then more data (``more_data.csv``) arrives and one realises the analysis script
needs to be able to work on more than one input file. However, rather than
editing the existing scripts one produces a new one to work on multiple files
(``multiple_file_analysis.py``).

```
$ tree some_project/
some_project/
├── analysis.py
├── data.csv
├── more_data.csv
├── multiple_file_analysis.py
├── output.csv
├── pre_process.py
└── cleaned_data.csv
```

Nine months later you run out of disk space. You need to delete unnecessary
files from an old project. Which files can you safely delete?


## A better way

Now suppose that you were faced with the same challenge of deleting unnecessary
files, but the files were organised into the directory structure below. Which
file(s) would you delete?

```
$ tree some_project/
some_project/
├── final_results
│   └── output.csv
├── intermediate_data
│   └── cleaned_data.csv
├── raw_data
│   ├── data.csv
│   └── more_data.csv
└── scripts
    ├── analysis.py
    ├── multiple_file_analysis.py
    └── pre_process.py
```

The directory names suggest that the intermediate file ``cleaned_data.csv`` is
a good candidate for deletion. If the whole analysis pipeline was automated one
may also consider deleting the final result (``output.csv``), depending on how
long it takes to run the analysis.

The setup above is an improvement. However it is not ideal. What if you, or one
of your colleagues, wanted to use the raw data for a different analysis. How
would you enable that?

### Avoiding data duplication

There are several reason to want to avoid duplicating data. One reason is
that every duplication costs disk space. Another reason is that having
multiple copies of data makes it ambiguous what the canonical source of the
data is.

A better solution is therefore to have a canonical place to store your raw data
in. For example something along the lines of the below.

```
$ tree research-group-share/
research-group-share/
├── projects
│   ├── cure_cancer
│   │   ├── final_results
│   │   ├── intermediate_data
│   │   └── scripts
│   └── feed_the_world
│       ├── final_results
│       ├── intermediate_data
│       └── scripts
└── raw_data
    ├── raw_data_1.csv
    ├── raw_data_2.csv
    └── raw_data_3.csv
```

In the above the all raw data is potentially available to all projects and it
is up to each project to specify which raw data files to include in its
analysis.

Having the data in a canonical location helps avoids data duplication. It also
has the added benefit of making it easier to find data.

### Summary

The key take home messages are:

1. Keep raw data separate from derived/intermediate data
2. Keep raw data in a consistent location
3. Standardise the structure of your projects so that it is easy to identify
   files that can be easily re-generated
