# Setting up your data science project

When setting up your data science project, it is incredibly important that you structure your project in a way that someone can drop into the project and immediately understand the structure and pick up where you left off. This person can in fact be yourself 6 months after you last worked on the project, so that is your intended user.

## TL;DR

`~/projects/dihi_qi/utils/init_ds_project.sh <NEW_PROJ_DIR>`

## Directory Structure

At DIHI, we use the following directory structure for our projects, which was inspired by https://github.com/Azure/Azure-TDSP-ProjectTemplate

```
.
├── data/
|   ├── assets/              # Contains things like groupers / crosswalks / columns names / etc.
|   ├── processed/           # Contains data that has been transformed from the raw data in any way
|   ├── modeling/            # Contains data that is used directly in the modeling phase of the project
|   └── raw/                 # Contains raw data that is produced by running the contents of the src/dataextract/ directory
├── models/                  # Any model artifacts -- can create subdirectories to store model predictions, etc.
├── notebooks/               # Jupyter notebooks for EDA/other explroatory stuff
├── src/                     # Source directory
|   ├── __init__.py           
|   ├── dataextract/         # Contains code to extract data
|   ├── dataprep/            # Contains code to prepare the data 
|   ├── modeling             # Contains model code -- hyperparameter sweeps, etc.
├── output/                  # Any output -- can contain things like reports/figures/etc.
```

This is generally a good starting structure for a project, but your specific project may require to slightly modify or add to this structure. This is ok as long as it is clear where certain things belong. If you are unsure about naming or adding directories, feel free to ask.

In a linux pace machine, you can bootstrap the environment by calling `~/projects/dihi_qi/utils/init_ds_project.sh <PATH_OF_NEW_PROJECT>`

### Relative Paths and Python Modules

When you are referring to files in your scripts/Jupyter Notebooks/etc., you should use relative paths in order to access files both within and outside your directory structure. For each of your scripts, assume that the your working directory is the *same* directory that your script is located in. For example, imagine that you have a script located at `PROJECT/Code/DataPrep/<yourscript>.py`, if you are reading in a data file at `PROJECT/Data/Raw/<datafile>.csv`, you would read it in as:

```python
import pandas as pd

df = pd.read_csv("../../Data/Raw/<datafile.csv")
```

This ensures that no matter who is accessing the script, it will run on their platform and machine. Note that `.` refers to the current directory and `..` refers to the parent level directory. 

Additionally, one common approach to python modules and running your code is to **run everything from the top-level directory**. For example, if I wanted to run something in the data extract folder, I would run

`python -m src.dataextract.myscript`

In order for this to work, you can import libraries using the following convention:

```
import settings
import src.utils as utils
from src.dataprep.somethingelse import some_func
```

This is often useful so that the `settings.py` file can sit at the top level and there is a consistent run path for all scripts (this makes it easier to deploy).

## Naming
When you name your files, make sure that you make it clear what the purpose of each file is. Avoid naming conventions such as adding `V2` or `V3` after the name, as this means nothing to someone else coming to your code. In addition, do not use date information such as when the script was created in the name of the script or in naming directories. In addition, do not use spaces in your directory naming. [snake_case](https://en.wikipedia.org/wiki/Snake_case) is preferred.
