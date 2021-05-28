# d4ad-standardization
Data 4 American Dream (D4AD) standardization code for innovateNJ contract. See: https://d4ad.com/ for more details.

## Instructions for standalone generation from `make_dataset.py`
These instructions produce a standardized version of D4AD provider data. First the environment must be set up (e.g. python environment, with libraries) and then a series of notebooks run.

## Setup/Assumptions
* This project is cloned, e.g. `git clone https://github.com/robinsonkwame/d4ad_standardization/`
* A current excel spreadsheet of D4AD provider data, with unstandardized columns of data, is copied to `.D4AD_Standardization/data/raw`, e.g. `etpl_all_programsJune3.xls`
* The `pipenv` package is available and how to install it [can be found here](https://docs.pipenv.org/)

## How to Run
```
# at ./d4ad_standardization
pipenv shell
pipenv install # wait a while for various packages to be installed

python D4AD_Standardization/src/data/make_dataset.py
# 
#2020-09-11 11:51:32,835 - __main__ - INFO - Making final data set from raw data
#2020-09-11 11:51:32,835 - __main__ - INFO - ... standardizing course names
#2020-09-11 11:51:39,271 - __main__ - INFO - ... standardizing provider names
#2020-09-11 11:51:39,891 - __main__ - INFO - ... standardizing abbreviations throughout ... will take a while ...
#2020-09-11 11:51:39,891 - __main__ - INFO - 	[abbreviation] starting at 2020-09-11 11:51:39.891658
#2020-09-11 11:54:20,586 - __main__ - INFO - 	[abbreviation] stopped at 2020-09-11 11:54:20.586612
#2020-09-11 11:54:20,586 - __main__ - INFO - 	[abbreviation] took 0:02:40.694954 time
#2020-09-11 11:54:20,586 - __main__ - INFO - ... identifying WIOA funded courses
#2020-09-11 11:54:25,351 - __main__ - INFO - ... identifying certficate courses
#2020-09-11 11:54:26,820 - __main__ - INFO - ... identifying associates
#2020-09-11 11:54:27,844 - __main__ - INFO - ... job search durations

#
# Now, examine the output standardized_etpl.csv file in ./D4AD_Standardization/data/interim/
# Standardized fields are prefixed with the word 'STANDARDIZED'; new fields (such as WIOA indicators) are
# new.
```

## TODOS

See any and all open issues.


--------

## Instructions for standalone generation from ipython notebooks
These instructions iteratively produce standardized version of D4AD provider data. First the environment must be set up (e.g. python environment, with libraries) and then a series of notebooks run. @kwame is currently refactoring the notebooks into a single source file that pulls in a set of modules so that this process is much simpler. But currently this is the procedure for running (note: only been tested on Kwame's machine, some revisions may be needed):

## Setup/Assumptions
* This project is cloned, e.g. `git clone https://github.com/robinsonkwame/d4ad_standardization/`
* A current excel spreadsheet of D4AD provider data, with unstandardized columns of data, is copied to `.D4AD_Standardization/data/raw`, e.g. `etpl_all_programsJune3.xls`
* The `pipenv` package is available and how to install it [can be found here](https://docs.pipenv.org/)

## How to Run
```
# at ./d4ad_standardization
pipenv shell
pipenv install # wait a while for various packages to be installed

# This `jupyter nbconvert ...` is a crude way to convert the notebook to a python script and then run it
# These scripts build off prior script outputs (found in ./data/interim) and must be run in order

# Generate a list of prerequisites, used to bootstrap abbreviation candidates, not needed
#jupyter nbconvert --to notebook --ExecutePreprocessor.timeout=-1 --execute ./D4AD_Standardization/notebooks/2.0-kpr-Abbreviations-DisqualifiedProviders-Content-Generation.ipynb

# Generate first standardization of the NAME field
jupyter nbconvert --to notebook --ExecutePreprocessor.timeout=-1 --execute ./D4AD_Standardization/notebooks/3.0-kpr-NAME.ipynb

# Generate first standardization of the NAME_1 (Program name) field, further standardization of the NAME field
jupyter nbconvert --to notebook --ExecutePreprocessor.timeout=-1 --execute ./D4AD_Standardization/notebooks/5.0-kpr-Progam_Course_NAME.ipynb

# Generate first standardization of description fields, IS_WIOA field
# note: this will take at least 3 minutes
jupyter nbconvert --to notebook --ExecutePreprocessor.timeout=-1 --execute ./D4AD_Standardization/notebooks/6.0-kpr-Description-with_Funding_type_degree_type_columns.ipynb

# Generate job search duration related fields 
jupyter nbconvert --to notebook --ExecutePreprocessor.timeout=-1 --execute ./D4AD_Standardization/notebooks/7.0-kpr-WIOA-field.ipynb
```

## Generated Datasets
The above commands should incremental generate the following .csv files
```
 42M Sep 10 12:11 with_job_search_durations.csv                           # 7.0 notebook
 42M Sep 10 12:08 standardized_descriptions_and_degree_funding_type.csv   # 6.0 notebook
 31M Sep 10 11:28 standardized_name_and_name1.csv                         # 5.0 notebook
 30M Sep 10 11:21 standardized_name.csv                                   # 3.0 notebook
5.7M Sep 10 11:16 state_comments.csv                                      # 2.0 notebook
 20M Sep 10 11:16 0_prereqs.csv                                           # 2.0 notebook
```

There are also `.xls` files in ./data/processed.
