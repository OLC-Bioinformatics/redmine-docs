## Publish Changes

Once you are satisfied with your new page, you need to commit your changes to the main and gh-pages branches of the repository.

1. First, open a new terminal window (another terminal than where you started the mkdocs preview server)

2. Then open the redmine-docs folder before doing the following steps
	- You can do this by using the shortcut `crtl-shift-t` on keyboard
	- Or by opening a new tab manually and navigating to the folder using `cd /mnt/nas2/redmine/redmine-docs`

3. **(OPTIONAL)** run `git status` to see confirm what needs to be added to the commit

	- `mkdocs.yml` should be present under the 'Changes not staged for commit' section
	- your new file (e.g. `docs/analysis/SISTR.md`) should be present under the 'Untracked files' section

4. Add your new file to the repository

	- e.g. `git add mkdocs.yml docs/analysis/SISTR.md`
	- tip: if you want to publish changes on multiple pages and want to repeat this line you can press the up arrowkey to copy your previous command
	
	![changes_screenshot](../img/publishing_changes_screenshot.png)
	
5. Commit your changes with a useful message

	- e.g. `git commit -m "Added SISTR analysis"`
	
6. Push your commits

	- `git push origin`
	
7. Use mkdocs to rebuild your site, and push the changes

	- `mkdocs gh-deploy`
	
	
If you have completed these steps successfully you should be given a message along the lines of _"https://olc-bioinformatics.github.io/redmine-docs/ should reflect the changes within a few minutes"_.


### Finding your Github Token

In order to push your changes, the terminal will request you enter your github credentials after the commands `git push origin` and `mkdocs gh-deploy`.

- Your username will be your email you used to sign up for Github or your Github username
- Your password will be your _github token_ NOT your Github password

You can generate a personal access token in your Github settings under `Developer Settings` > Personal access tokens > Tokens (classic). Select an expiration date and your scopes (ie. what you want your token to give access to) then click `Generate token`.

Your token will be a string of characters that you will (probably) not remember, so be sure to record it in a secure document.

**Note:** the terminal will not show your password as you type it and using the `ctrl-v` shortcut won't work in the terminal so right-click and paste instead

