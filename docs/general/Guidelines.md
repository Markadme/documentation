# Guidelines

## Project Structure

```
project
│   README.md   
|
└── doc
└── src
|   |   __init__.py
|   |
│   └── nodes
|       |   __init__.py
|       |
│       └── data_sources
|       |   |   __init__.py
|       |
│       └── data_processors
|       |   |   __init__.py
|       |
│       └── end_nodes
|           |   __init__.py
│   
└── scripts
└── launch
└── (windows_scripts)
└── testing
```

`doc` contains all files related to the documentation (Readme) \
`src` contains the actual project files. Everything in this folder is declared to a package by the `__init__.py` to allow a uniform import \
`src/nodes` Contains all modules that can be connected through the framework \
`src/nodes/data_sources` Contains all data sources \
`src/nodes/data_processors` Contains all data processors \
`src/nodes/end_nodes` Contains all end nodes \
`scripts` Contains all scripts that can be executed with ros \
`launch` Contains all launch scripts for ros \
`windows_scripts`  Contains all scripts that can be executed in windows without ros \
`testing` Contains all testcases

## Naming

## Git guidelines

?

## Testing

?
