## Create a new page

Ensure that the repository is up-to-date:

- if you don't have a local copy of the repository, clone a fresh version using `git clone https://github.com/OLC-Bioinformatics/redmine-docs.git`

- or update an existing copy with `git pull origin master` from within the repository

Create a page:

- Open `redmine-docs/mkdocs.yml` in the redmine-docs folder (nas2/redmine/redmine-docs) with your favourite text editor

- Decide where you wish to place the new page 

- Add a line with the desired name and location of the analysis

	- e.g. if you want to add SISTR, include the following line `- 'SISTR': 'analysis/SISTR.md'`
	- make sure you following the spacing conventions, and that the list is still alphabetical) to the 'Analysis' category
	
	![file_screenshot](../img/mkdocs.yml_screenshot.png)
	
- Create a markdown file in the location specified in mkdocs.yml
	- All the files for the pages are in the `docs` folder (…/redmine/redmine-docs/docs)
	- So, in the above example, you would need to create `docs/analysis/SISTR.md` in the redmine-docs repository
	- Ashley usually uses `touch` to accomplish this: `touch docs/analysis/SISTR.md` if she is in the root of the repository
	- I usually just duplicate another analysis markdown file already in the category I want so I can use it as a template
