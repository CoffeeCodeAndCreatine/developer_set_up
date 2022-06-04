# Mac Studio Developer Set Up

## Intro
The purpose of this document is to go through my setup process for development on an untouched Mac.
By the end of this document you will have a machine that is set up for python, javascript and java development.
We will also install a suite of tools along the way such at git and iterm to make development a little more stream lined.
It should be noted that this document will cover the set up of an untouched Mac Studio (Ultra) running Monterey 12.4.

In the event you are setting up an intel based mac, I have a playlist of videos here that will walk you though a very similar process.

* [Part 0: Summary](https://www.youtube.com/watch?v=Lgp8fwM-M38)
* [Part 1: The Basics](https://www.youtube.com/watch?v=8TZYsQkhXwc&t=4s)
* [Part 2: IDEs](https://www.youtube.com/watch?v=rMD1JW6DpkM)
* [Part 3: Developer Convenience Apps](https://www.youtube.com/watch?v=tiwwI1zw6ng)
* [Part 4: Developer Apps](https://www.youtube.com/watch?v=2yQd-2uUzao)
* [Part 5: Developer Set Up](https://www.youtube.com/watch?v=hXfegfi824k)


## Before We Start
Going from intel to m1 can be a bit of a trick. Some tools and set up work well for intel and do not for m1. Other apps are optimized for m1 while other will require rosetta (For apps that are m1 optimized please see [Apps](APPLICATION.md)).
This set up, should get you a clean environment with optimized apps, however it will require a few steps done in specific order, and a few edits to profiles and paths.
The profiles, assuming the mac in untouched will not yet be created. A base path will look like this.

**Basic Path**
```bash
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin
```

In the event something goes sideways, we can always revert the path back to stock.
In order to print your path, run the following from your terminal.
```bash
echo $PATH
```

## Step 1: XCode: DO NOT SKIP
### XCode
This is where a bit of magic comes in. XCode is Apple's IDE and the installation and boot of this app will get you xcode as well as all of xcode's developer and command line tools. We will need these later, and if you try to do some steps without these tools you can and will end up in an unstable state.
As such, step one is to install XCode from the app store. It's about a 12 gig download and takes a bit to install. Get it started, and go get a coffee.

## Step 2: Homebrew
Apple machines come without a package manager, so we are going to install one. There are a few out in the world, however homebrew has been around for a while, works well from a "fire and forget" sense and "developer" sense, and has been optimized for m1 chips.
To install home brew, run the following three commands from your terminal.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/jacobdaigle/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Going down the list of commands, the first installs homebrew. The second adds a line to your zprofile to initialize homebrew on start. The third line initialized homebrew for your current session.

In the event you will like a little extra reading on homebrew, you can explore the following links.
* [Homebrew Docs](https://docs.brew.sh/)
* [Homebrew For M1](https://mac.install.guide/homebrew/index.html)

Once you have installed homebrew, your path will have changed. If everything went to plan, home brew will be at the front of your path. This is exactly what we want, and should look something like this.

**Homebrew Path**
```bash
/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin
```

### Step 3: ZSH
For those who know, they know. Zsh is a bit nicer than pure bash. So let's get that installed. We will do so from the terminal using brew.
Run the following command from your terminal.
```bash
brew install zsh
```

### Step 4: Oh My Zsh
Zsh is great, but Oh My Zsh gets us quite a few dev tools and nice UI tweaks. Run the following command via your terminal.
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
This command uses curl to download and run an install script.

In the event you would like a little extra reading on iterm, you can find their main page [here](https://ohmyz.sh/#install).

### Step 5: Iterm2
Terminal is nice, it does what it claims to do, but a lot could be added. This is where iterm2 comes in. This is an app, so no command line here. Just go to their site and install.
A dmg for Iterm2 can be found [here](https://iterm2.com/).



### Step 5.1: Aesthetics
At this point I would suggest taking a break and installing a few nice to haves to make terminal a little cleaner.
These will all be aesthetic changes only, but will make your life a little easier.

#### Step 5.2 Oh My Zsh Theme
With the install of Oh My Zsh you will have gotten a zshrc file. This file can be printed out from the terminal with the following command.
```bash
cat ~/.zshrc
```
In this file, you are going to see a line looking something like this
```bash
ZSH_THEME="robbyrussell"
```
I would suggest changing this to agnoster. Change the above line to this
```bash
ZSH_THEME="agnoster"
```
This can be done via your favorite text editor.

#### Step 5.3: Fonts
If you look at your terminal you are probably going to see a few characters that are not rendering correctly. They are going to look like little boxes. This is because we don't have the fonts that the theme wants. Let's intall those now.
This process will include downloading a repo, installing fonts, and then setting a font in iterm.
Cloning the repo can be down by running the following command in your terminal
```bash
git clone https://github.com/powerline/fonts.git
```
After cloning the repo you need to run the install script from your terminal.
```bash
cd fonts
./install
```
This will install all the fonts into your system. From here, all you have to do is pick a font in iterm. This can be done by opinion the preferences (command + ,), going to profile, and then text.
I typically use `Meslo LG S for Powerline`.

#### Step 5.4: Colors
The base color scheme of iterm is nice, but the black on blue can be a bit tough to read. You can use another base color scheme of `Tango Dark` which solves this problem, but I like options.
Cloning this repo will get you quite a few color options.
```bash
git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
```
Once you have this repo cloned, you can open the preferences of iterm, go to profiles and then colors, hit the drop down, select import and select the color theme of choice to import.
The correct files for iterm will be found at `iterm2-Color-Schemes/schemes`

### Step 6: Installing Python
For python development we really don't want to use our system version. In the event we mess something up in terms of build or packages, it can be quite painful to get everything cleaned back up. To prevent this, we are going to use virtual environments, I generally like to use pyenv.
We will be installing pyenv via our terminal though home brew.
```bash
brew install pyenv
```

Once you have pyenv installed we will need to add it to your profile and source the profile. Your machine should have a file `~/.zprofile`. Add the following lines to the file.
```bash
#Pyenv Set Up
export PYENV_ROOT=$HOME/.pyenv
eval "$(pyenv init -)"
```
and resource the file
```bash
source ~/.zprofile
```

Once you installed and sourced your new profile, you are going to get something called shims added to your path.

**Pyenv Path**
```bash
/Users/jacobdaigle/.pyenv/shims:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin
```
This is where pyenv is going to store python and allow us to access it. This needs to be at the front of your path, otherwise you will end up using your system python, not shims python.

Once we have pyenv installed, we are going to want to install a version of python, I would suggest using latest, at the time of writing it was 3.10. Then we are going to want to set that version of python to be our global version. That can be done view the following two commands.
```bash
pyenv install 3.10.4
pyenv global 3.10.4
```

Then, if you run `python --version` you should get 3.10.4. 
### Step 7: Installing Java
coming soon
### Step 8: Installing JavaScript
Getting set up with javascirpt is pretty straight forward as of this point. The setup we are going to use is nvm (node version manager) to install node. Then we will use nvm to install both node and react.
To install nvm and create a nvm dir run the following commands from your command line.
```bash
brew install nvm
mkdir ~/.nvm
```

Once you have nvm installed, you are doing to need to add a few things to your zprofile. Add the following lines.
```bash
#NVM Set Up
export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

Once you have edited the zprofile, you will need to resource it.
```bash
source .zprofile
```

Once you have sourced your new profile, your path will now look something like this.

**Node Path**
```bash
/Users/jacobdaigle/.nvm/versions/node/v18.3.0/bin:/Users/jacobdaigle/.pyenv/shims:/opt/homebrew/bin:/opt/homebrew/sbin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin
```

Notice nvm is now on your path.

From here we should be able to install node, and react. That can be done with the following two commands. These will install latest node and react.
```bash
nvm install node
npm install react --save
```



### Step 9: Git Hub
Most likely if you are writing code you are using some version of source control. If you are using git, you are going to need to add an ssh key to git.
In order to generate one on your local machine, run the following two commands
```bash
ssh-keygen -t ed25519 -C "YOUR EMAIL"
pbcopy < ~/.ssh/id_ed25519.pub
```
This will create your key, and then copy your public key to your clipboard. 
Then you can add it to your github account [here](https://github.com/settings/keys)

For further reading on github keys, you can reivew the following documents.
* [Generating an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
* [Adding and SSH key to git](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)