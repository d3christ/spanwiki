# Contributing

If you are a member of the lab and see something wrong or out of date with an existing section, or wish to add your own new section, then you should contribute! 

First, talk to the current lab manager to get the login information for the github spanlab account, or (even better) have the admin to make you a collaborator. 

Second, edit the relevant .md file, or make a new one. Looking at the raw version of any of the files should make it pretty clear how markdown works. 

## Editing online (existing files)
Assuming you have push access and want to edit an existing file, once you click on any markdown file, you should see a little pencil icon at the top right of the text box. This should drop you into an editor, and you can click 'commit changes' at the bottom when you're done to save your edits. It's usually helpful to include a small description of your changes if you're changing a lot, so that we can see what's changed in the git log. 

## Making a new file

## Editing locally
To make changes locally, you'll need to:
1. Be a contributor/spanlab and be logged into git locally with your username.
2. Clone this repository (download a local copy) with the command `git clone https://github.com/spanlab/spanwiki.git`
3. Make all the local changes you'll need. 
4. Add any new files with `git add *`
5. Commit your changes (save them locally, but not online): `git commit -am "Include a description here." `
5.5 do a `git pull`, since things may have changed on the server since you started editing. If you see that there are now conflicts, [fix them like so](https://stackoverflow.com/questions/161813/how-to-resolve-merge-conflicts-in-git)
6. Push your changes (you have to commit first) `git push origin master`


## Useful links:
[Basic git tutorial](http://rogerdudler.github.io/git-guide/)
[Markdown syntax reference](http://commonmark.org/help/)
