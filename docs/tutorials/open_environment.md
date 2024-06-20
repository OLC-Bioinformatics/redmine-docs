## Opening a mkdocs conda environment

#### Create a fresh conda environment

`conda create -n mkdocs`

Activate the environment

`conda activate mkdocs`

Install mkdocs

`conda install mkdocs`

Install the bootswatch package, so that the flatly theme works

`pip install mkdocs-bootswatch`

####If a conda is already installed

Change directories to open the redmine docs folder

`cd /mnt/nas2/redmine/redmine-docs`

>_Tip! If  you want to ensure that you are in the correct folder enter 'ls' into the terminal. The system will list all the folders under the directory you are currently under._

Activate the conda

`conda activate /mnt/nas2/virtual_environments/mkdocs`
