## Publish Changes

Once you are satisfied with your new page, you need to commit your changes to the main and gh-pages branches of the repository 

- **(OPTIONAL)** run `git status` to see confirm what needs to be added to the commit

	- `mkdocs.yml` should be present under the 'Changes not staged for commit' section
	- your new file (e.g. `docs/analysis/SISTR.md`) should be present under the 'Untracked files' section

- add your new file to the repository

	- `git add mkdocs.yml docs/analysis/SISTR.md`
	
- commit your changes with a useful message

	- `git commit -m "Added SISTR analysis`
	
- push your commits

	- `git push origin master`
	
- use mkdocs to rebuild your site, and push the changes - https://olc-bioinformatics.github.io/redmine-docs/ should reflect the changes within a few minutes

	- `mkdocs gh-deploy`
