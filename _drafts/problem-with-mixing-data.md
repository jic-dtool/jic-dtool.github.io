---
layout: post
title: The problem with mixing data
---

Practical advice on how to structure files and directories in a project.

You have run out of space in your research group share. You need to delete
unnecessary files. What command would you use to delete the unnecessary files
in the directory structure below?

```
$ tree my_project/
my_project/
├── analysis.py
├── data.csv
├── more_data.csv
├── multiple_file_analysis.py
├── output.csv
├── pre_process.py
└── processed_data.csv
```

When one starts working on a project it is not uncommon to keep scripts close
to the raw data. Usually, this is as a result of (short term) convenience.

A project may therefore start out like this:

```
$ tree my_project/
my_project/
└── data.csv
```

Then one adds a script to process the data:

```
$ tree my_project/
my_project/
├── analysis.py
    └── data.csv
```

Then one runs the analysis script on the data:

```
$ tree my_project/
my_project/
├── analysis.py
├── data.csv
└── output.csv
```

Then one creates another script to pre process the data and run it:

```
$ tree my_project/
my_project/
├── analysis.py
├── data.csv
├── output.csv
├── pre_process.py
└── processed_data.csv
```

Then some more data arrives and one realises the analysis script needs to be
able to work on more than one input file:

```
$ tree my_project/
my_project/
├── analysis.py
├── data.csv
├── more_data.csv
├── multiple_file_analysis.py
├── output.csv
├── pre_process.py
└── processed_data.csv
```

You have run out of space in your research group share. You need to delete
unnecessary files. What command would you use to delete the unnecessary files
in the directory structure below?

```
$ tree my_project/
my_project/
├── final_results
│   └── output.csv
├── intermediate_data
│   └── processed_data.csv
├── raw_data
│   ├── data.csv
│   └── more_data.csv
└── scripts
    ├── analysis.py
    ├── multiple_file_analysis.py
    └── pre_process.py
```

The setup above is an improvement. However it is not ideal. What if you, or one
of your colleagues, wanted to use the raw data for a different analysis. How
would you enable that?

**Copying the raw data is bad!**

Making a symbolic link to the raw data would mean that the location and the
directory structure of the current project could never change. This is not
ideal.

A better solution is to have a canonical place to store your raw data in.
For example something along the lines of the below:

```
$ tree research-group-share/
research-group-share/
├── projects
│   ├── abc_proj
│   │   ├── final_results
│   │   ├── intermediate_data
│   │   └── scripts
│   └── def_proj
│       ├── final_results
│       ├── intermediate_data
│       └── scripts
└── raw_data
    ├── raw_data_1.csv
    ├── raw_data_2.csv
    └── raw_data_3.csv
```
