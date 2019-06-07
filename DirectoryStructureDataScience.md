# Setting up your data science project

When setting up your data science project, it is incredibly important that you structure your project in a way that someone can drop into the project and immediately understand the structure and pick up where you left off. This person can in fact be yourself 6 months after you last worked on the project, so that is your intended user.

## Directory Structure

At DIHI, we use the following directory structure for our projects, which was inspired by https://github.com/Azure/Azure-TDSP-ProjectTemplate

```
.
├── Code/
|   ├── DataPrep/
|   └── Model/
├── Docs/
|   ├── Data/
|   └── Project/
├── Data/
|   ├── Raw/
|   └── Processed/
|   └── Modeling/
├── Output/
|   └── Figures/
```

This is generally a good starting structure for a project, but your specific project may require to slightly modify or add to this structure. This is ok as long as it is clear where certain things belong. If you are unsure about naming or adding directories, feel free to ask.

If you are in a Windows PACE machine, you will find a script called `create_project.bat`. If you are starting a new project, double click the icon and it will prompt you for a project name and then generate all of the folders for you to get you started.

### Relative Paths

When you are referring to files in your scripts/Jupyter Notebooks/etc., you should use relative paths in order to access files both within and outside your directory structure. For each of your scripts, assume that the your working directory is the *same* directory that your script is located in. For example, imagine that you have a script located at `PROJECT/Code/DataPrep/<yourscript>.py`, if you are reading in a data file at `PROJECT/Data/Raw/<datafile>.csv`, you would read it in as:

```python
import pandas as pd

df = pd.read_csv("../../Data/Raw/<datafile.csv")
```

This ensures that no matter who is accessing the script, it will run on their platform and machine. Note that `.` refers to the current directory and `..` refers to the parent level directory. 

## Naming
When you name your files, make sure that you make it clear what the purpose of each file is. Avoid naming conventions such as adding `V2` or `V3` after the name, as this means nothing to someone else coming to your code. In addition, do not use date information such as when the script was created in the name of the script or in naming directories. In addition, do not use spaces in your directory naming. [snake_case](https://en.wikipedia.org/wiki/Snake_case) is preferred.