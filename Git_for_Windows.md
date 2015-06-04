# Introduction #

To obtain a copy of the project files, you'll either want to use the download page for the official releases, or if you want the bleeding edge, you'll need to obtain a copy of the git repo. The below describes how to obtain and keep your copy of the repo up to date with the online git repo.




# What you need (note tortisegit does not support win2k and older) #

1) Install [msysgit - Git for Windows](http://code.google.com/p/msysgit/downloads/detail?name=Git-1.7.7-preview20111014.exe&can=3&q=official+Git/)

> most people will want to just accept all the default choices during the install

2) Install EITHER

  * [32 bit tortisegit](http://code.google.com/p/tortoisegit/downloads/detail?name=TortoiseGit-1.7.4.0-32bit.msi&can=2&q=/)

  * [64 bit tortisegit](http://code.google.com/p/tortoisegit/downloads/detail?name=TortoiseGit-1.7.4.0-64bit.msi&can=2&q=/)

> most people will want to just accept all the default choices during the install

# How to use it #

## Create a Clone (get the code onto your computer) ##

1) create a folder where you want to put you project (right click, new folder)

2) Right click the folder and select "Git "

3) Right click the folder again and select "Git clone"

> 3a)In the top box url, you need an address to enter "https://code.google.com/p/open5xxxecu.firmware/"

> 3b)The second box Directory should fill itself in, but this is just the location of the folder you want to use if it doesn't

> 3c)Click "OK" at the bottom


## Updating your local copy  - Git Pull ##

> You'll probably want to keep your local copy up to date, so to update you need to do a Git pull:

  1. Right click the your project folder and select "TortoiseGit"

> 2) Select "Pull"

> 3) Click  "OK" (you have an option to chose which branch if there are multiples)

## Creating a Branch ##

> It a good idea to create a branch for changes.

  1. Right click the your project folder and select "TortoiseGit"

> 2)  Select "Create Branch"

> 3)  pick a name and enter it in the window.

> 4) click "OK" at the bottom

## Switching to a Branch ##

To use your new branch

  1. Right click the your project folder and select "TortoiseGit"

> 2)  Select Switch/Checkout

> 3)  Pick a name or the branch you want to work on.

> 4) click "OK" at the bottom

## Commit ##

> Once you've may changed to anything you'll want to save them, a commit is how you do that.

  1. Right click the folder and select "Git Commit" (note it will show the branch you are working on)

> 2) The first time you do a commit you will be asked to enter a user name and email.



# Alternate Using msysgit without Tortisegit #

## Windows repo clone with msysgit ##

1. Obtain and install a copy of [msysgit - Git for Windows](http://code.google.com/p/msysgit/downloads/detail?name=Git-1.7.7-preview20111014.exe&can=3&q=official+Git/)

2. Open "Git GUI" and choose "Clone Existing Repository"

3. Choose the repo you want to clone, this one was for the hardware repo.

[http://wiki.open5xxxecu.googlecode.com/git/Git\_GUI\_clone-wiki.PNG](http://wiki.open5xxxecu.googlecode.com/git/Git_GUI_clone-wiki.PNG)

That's it, you now have the most recent copy of the repo. Open up the folder and browse around, see what's there and ask questions on the forum.

# Keeping up to date with windows and the git repo via msysgit #

1. You aleady have msysgit installed, so open git gui.

2. Open an existing.

3. Under remote, choose fetch from origin. This will obtain the latest changes from the web sourced that was originally cloned. However it does not apply those changes to your local repo, it just obtains the changes.

4. Under Merge, choose Merge local. This will then merger those changes that were downloaded in the above step.

You should how have the most recent copy of the repo.

# How to push if your a developer #

1. Open Git GUI and choose an existing repo. It will open right away, and after some time it will show new files in the top left staged area of the GUI.

2. Via pull downs, choose commit --> stage commit. This will tell Git GUI what files you want to include in the commit.

3. Enter some notes in the bottom right box, then hit sign off.

4. Now hit commit. This will commit the changes and the notes to your local repo.

5. If you want to share these changes, you'll want to push your changes up to a non local repo. In this case that's the Google code page. There are some things you'll need for that, first is to have been granted access to the online repo. This will provide you with a log in and password. In my case my log in is my e-mail address that looks something like this ?????@gmail.com. The password is found on one of the preferences pages when I'm logged in. Also via preferences page, I can choose use my Google code password instead of the random generated number they provide. Now when I hit push, I specify what to push, branch master, origin, ect. It will ask me for my user, and password. If all goes well it will provide a screen that notes success in a green bar. If it does not go well, you'll get an error message that doesn't tell you much.





