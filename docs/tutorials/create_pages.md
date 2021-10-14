## Create a new page

Open `redmine-docs/mkdocs.yml` in the redmine-docs repository

Decide where you wish to place the new page 

- if you want a new analysis, update the file with the desired name and location of the analysis

	- e.g. if you want to add SISTR, include the following line `- 'SISTR': 'analysis/SISTR.md'` (make sure you following the spacing conventions, and that the list is still alphabetical) to the 'Analysis' category
	
Create an empty markdown file in the location specified in mkdocs.yml - the files are in the `docs` folder, so, in the above example, you would need to create `docs/analysis/SISTR.md` in the redmine-docs repository (I usually use `touch` to accomplish this: `touch docs/analysis/SISTR.md` if I'm in the root of the repository)
