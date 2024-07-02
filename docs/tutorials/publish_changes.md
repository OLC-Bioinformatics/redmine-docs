## Publish Changes

Once you are satisfied with your new page, you need to commit your changes to the main and gh-pages branches of the repository 

- **(OPTIONAL)** run `git status` to see confirm what needs to be added to the commit

	- `mkdocs.yml` should be present under the 'Changes not staged for commit' section
	- your new file (e.g. `docs/analysis/SISTR.md`) should be present under the 'Untracked files' section

- add your new file to the repository

	- `git add mkdocs.yml docs/analysis/SISTR.md`
	
- commit your changes with a useful message

	- `git commit -m "Added SISTR analysis"`
	
- push your commits

	- `git push origin`
	
- use mkdocs to rebuild your site, and push the changes - https://olc-bioinformatics.github.io/redmine-docs/ should reflect the changes within a few minutes

	- `mkdocs gh-deploy`
	
### Finding your Github Token

In order to push changes, the terminal will request you enter your github credentials after the commands `git push origin` and `mkdocs gh-deploy`.

- Your username will be your email you used to sign up for Github or your Github username
- Your password will be your _github token_ NOT your Github password

You can generate a personal access token in your Github settings under `Developer Settings` > Personal access tokens > Tokens (classic). Select an expiration date and your scopes (ie. what you want your token to give access to) then click `Generate token`.

Your token will be a string of characters that you will not remember, so be sure to write it in a secure document.

Note: the terminal will not show your password as you type it.

