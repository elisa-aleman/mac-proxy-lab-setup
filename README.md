# Mac Python setup environment for Machine learning Laboratory on a proxy

This is how I set up a fresh mac to start working in machine learning and programming. My lab runs under a proxy, so all the settings have that one extra ...sigh... task to work correctly. I keep this tutorial handy in case I do a clean OS install or if I need to check some of my initial settings.

<!-- MarkdownTOC -->

- [Basic Settings](#basic-settings)
- [Setup proxy system wise](#setup-proxy-system-wise)
    - [Normal settings](#normal-settings)
    - [Time settings](#time-settings)
- [Setup proxy settings in bash](#setup-proxy-settings-in-bash)
- [Install Homebrew](#install-homebrew)
    - [Install Xcode Command Line Tools](#install-xcode-command-line-tools)
    - [Install Homebrew under a proxy](#install-homebrew-under-a-proxy)
- [Curl proxy settings](#curl-proxy-settings)
- [Install Python](#install-python)
    - [Install useful python libraries](#install-useful-python-libraries)
- [Install and setup Git](#install-and-setup-git)
    - [Install Git Large File System](#install-git-large-file-system)
    - [Make a new Git \(LFS\) repository from local](#make-a-new-git-lfs-repository-from-local)
    - [Check your branches in git log history in a pretty line](#check-your-branches-in-git-log-history-in-a-pretty-line)
- [Install and setup Ruby, Bundler and Jekyll for websites](#install-and-setup-ruby-bundler-and-jekyll-for-websites)

<!-- /MarkdownTOC -->


## Basic Settings

Setup root password:
https://www.cyberciti.biz/faq/how-to-change-root-password-on-macos-unix-using-terminal/

```
sudo passwd root
```

Hidden files visibility:
I can't work without seeing hidden files, so in the newer versions of MacOSX we can do `CMD+Shift+.` and the hidden files will appear. 
If this doesn't work, open up the terminal and:
```
defaults write com.apple.finder AppleShowAllFiles YES
```

## Setup proxy system wise

### Normal settings

First, know the {PROXY_HOST} url and the {PORT} that you need to access in your specific place of work. Then put those in the system settings as appropriate.

`Settings > Network > Advanced > Proxies`

Web Proxy (HTTP)
`{PROXY_HOST}:{PORT}`

Secure Proxy (HTTPS)
`{PROXY_HOST}:{PORT}`

FTP Proxy
`{PROXY_HOST}:{PORT}`

Bypass proxy settings for these Hosts & Domains:
`*.local, 169.254/16, 127.0.0.1, HOST_URL`

Depending on your organization, setting a HOST_URL to bypass here might also be necessary. Check with your administrator.

### Time settings

Some proxies need you to set up the time of the computer to match the network in order to work correctly. Mine does at least. So the settings are such:

Settings > Date & Time > Date & Time

Set Date and Time automatically:
{TIME_URL}

The {TIME_URL} will depend on your organization, so check with your administrator.

## Setup proxy settings in bash

So now that the system settings are out of the way, we need to setup the proxy addresses in bash so that programs we run take those variables, since the system setup doesn't reach deep enough for several tools we use.

So we will add these settings to `.zprofile` to load each time we login to our user session. 

```
sudo nano .zprofile
```

This will open a text editor inside the terminal. If the file is new it will be empty.
Add the following lines: (copy and paste)

```
export http_proxy={PROXY_HOST}:{PORT}
export https_proxy={PROXY_HOST}:{PORT}
export all_proxy={PROXY_HOST}:{PORT}
export HTTP_PROXY={PROXY_HOST}:{PORT}
export HTTPS_PROXY={PROXY_HOST}:{PORT}
export ALL_PROXY={PROXY_HOST}:{PORT}
export no_proxy=localhost,127.0.0.1,169.254/16,HOST_URL
export NO_PROXY=localhost,127.0.0.1,169.254/16,HOST_URL
```

Then these ones for logging in ssh to the servers in the laboratory without typing it every time.

```
alias lab_server="ssh -p {PORT} {USERNAME}@{HOST_IP_ADDRESS}"
```

Of course, you'll need your own {PORT} and {USERNAME} and {HOST_IP_ADDRESS} here, depending on where you want to log in.

Press `CTRL+O` to write, press `ENTER` to keep the name, then press `CTRL+X` to close the editor

Relaunch the terminal.

## Install Homebrew

For more info, click [here](https://brew.sh).

First we need to consider the macOS Requirements from their website:

- A 64-bit Intel CPU
- macOS High Sierra (10.13) (or higher)
- Command Line Tools (CLT) for Xcode: `xcode-select --install`, [developer.apple.com/downloads](https://developer.apple.com/downloads) or [Xcode](https://itunes.apple.com/us/app/xcode/id497799835)
- A Bourne-compatible shell for installation (e.g. bash or zsh)

As it says in the third requirement, we need the Command Line Tools for Xcode.

### Install Xcode Command Line Tools

We have 3 options:

- Install just the tools:

```
xcode-select --install
```

- Download them from the official Apple website

Go to [developer.apple.com/downloads](https://developer.apple.com/downloads) and sign in with your Apple ID and password.

Agree to the Apple Developer Agreement.

Select the latest non-beta Command Line Tools for Xcode available, download de .dmg file and install.

- Install the full Xcode app

Xcode as an app is really heavy, so if you don't intend to work directly on the IDE of Xcode or on any other uses of the app, I don't recommend it. I also have no experience with setting up the CLT with this path.

For this option, you also need to sign up to be an Apple Developer. 

### Install Homebrew under a proxy

Now that we have the CLT, we can proceed.

First configure your git proxy settings:
```
git config --global http.proxy http://{PROXY_HOST}:{PORT}
```
Replace your {PROXY_HOST} and your {PORT}.

Then install homebrew using proxy settings as well:
```
/bin/bash -c "$(curl -x {PROXY_HOST}:{PORT} -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

And finally alias `brew` so it always uses your proxy settings:

```
alias brew="https_proxy={PROXY_HOST}:{PORT} brew"
```

Otherwise, if you're not under a proxy just follow the instructions here:
https://docs.brew.sh/Installation

## Curl proxy settings

Right there installing Homebrew we used explicit proxy settings on the curl command to avoid any issues, but to avoid doing this every time for future uses of curl, we also need to setup proxy settings.

```
sudo nano ~/.curlrc
```

And add the following:
```
proxy = {PROXY_HOST}:{PORT} 
```

## Install Python

Mac already has a system used python but it is better to avoid using it as it interacts with the system, so we install a local version with Pyenv. Pyenv also makes it so that pip and python are always matched for each other in the correct version.

This is specially useful if you need different versions for different projects (Maybe caused by tensorflow updates vs other libraries updates...), you should follow these tutorials:

https://opensource.com/article/19/5/python-3-default-mac
https://opensource.com/article/20/4/pyenv


```
brew install pyenv
```

Now let's install and set the latest version:
```
pyenv install 3.9.5
pyenv global 3.9.5

```

And then we can add it to our PATH so that everytime we open `python` it's the pyenv one and not the system one:

```
echo 'PATH=$(pyenv root)/shims:$PATH' >> ~/.zprofile
source ~/.zprofile
```

We can confirm we are using the correct one:

```
pyenv versions
which python
python -V
which pip
pip -V
```

### Install useful python libraries

This is my generic fresh start install so I can work. There's more libraries with complicated installations in other repositories of mine, and you might not wanna run this particular piece of code without checking what I'm doing first. For example, you might have a specific version of Tensorflow that you want, or some of these you won't use. But I'll leave it here as reference.

```
pip install numpy scipy statsmodels matplotlib more_itertools pandas sklearn beautifulsoup4 retry requests selenium gensim nltk langdetect sympy tqdm pyclustering tensorflow tflearn minepy keras
```

## Install and setup Git

Mac already has git installed (that's how we installed homebrew too), but we can update it and manage it with homebrew.

```
brew install git
```

Then setup the configuration. Make an account at [GitHub](https://github.com) to get a username and email associated with Git. Type the settings on the terminal. My settings are like this:

```
git config --global http.proxy http://{PROXY_HOST}:{PORT}
git config --global user.name {YOUR_USERNAME}
git config --global user.email {YOUR_EMAIL}
git config --global core.editor nano
git config --global merge.conflictstyle diff3
git config --global color.ui auto
git config --global core.autocrlf input
git config --global pull.ff only
```

This should make a file .gitconfig with the following text
```
[http]
    proxy = http://{PROXY_HOST}:{PORT}
[user]
    name = YOUR_USERNAME
    email = YOUR_EMAIL
[color]
    ui = auto
[merge]
    conflictstyle = diff3
[core]
    editor = nano
    autocrlf = input
[pull]
    ff = only
```

Now to make your Mac ignore those pesky Icon? files in git:

The best place for this is in your global gitignore configuration file. You can create this file, access it, and then edit per the following steps (we need to use vim because of a special character in the Icon? files):

```
git config --global core.excludesfile ~/.gitignore_global
vim ~/.gitignore_global
```

press `i` to enter insert mode
type `Icon` on a new line
while on the same line, ctrl + v, ENTER, ctrl + v, ENTER
Also, let's add `.DS_Store` to .gitignore_global as well.

press ESC, then type `:wq` then hit ENTER.

### Install Git Large File System

This is for files larger than 50 MB to be able to be used in Git. Still, GitLFS has some limitations if you don't buy data packages to increase your usage limit. By default you get 1GB of storage and 1GB of bandwidth (how much you push or pull per month). For 5$USD, you can add a *data pack* that adds 50GB bandwith and 50GB Git LFS storage.

Now we need to install the git-lfs package to use it:

```
brew install git-lfs
```

Now I personally had issues with git-lfs not pushing or pulling because of my proxy.

This was fixed by checking my exported variables in `.zprofile`.

The problem was it was set up like this:

```
export https_proxy=http://{PROXY_HOST}:{PORT}
export HTTPS_PROXY=https://{PROXY_HOST}:{PORT}
```

Where the proxy host at my lab doesn't manage the `https:// ` addresses correctly. So I had to correct them and remove the `s` like this:

```
export https_proxy=http://{PROXY_HOST}:{PORT}
export HTTPS_PROXY=http://{PROXY_HOST}:{PORT}
```

So the `https_proxy` variables still point to a `http://` address. It's not the best but in my network there was no other choice. 

### Make a new Git (LFS) repository from local

Now that we have Git and Python installed, we can make our first project. I like to leave this part of the tutorial in even if it doesn't classify as a setup because using Git and GitLFS was confusing at first.

First make a repository on GitHub with no .gitignore, no README and no license.
Then, on local terminal, cd to the directory of your project and initialize git
```
cd path/to/your/project
git init
```

If using Git LFS:
```
git lfs install
```
It's supposed to be ready, but first, let's make a few hooks executable
```
chmod +x .git/hooks/*
```

Make a .gitignore depending on which files you don't want in the repository and add it
```
git add .gitignore
```

If using Git LFS, add the tracking settings for this project (For example, heavy csv files in this case)
```
git lfs track "*.csv"
```

And then add them to git
```
git add .gitattributes
```

Commit these changes first
```
git commit -m "First commit, add .gitignore and .gitattributes"
```

Now add all the data from your local repository. `git add .` adds all the files in the folder.
```
git add .
```

Depending on the size of your project, it might be wiser to add it in parts instead of all at once. e.g.
```
git add *.py
git add *.csv
...
```
or
```
git add dir1
git add dir2
...
```

Check if all the paths are added
```
git status
```

Check if all the Git LFS files are tracked correctly
```
git lfs ls-files
```

If so, commit.
```
git commit -m "First data commit"
```

Set the new remote URL from the repository you created on GitHub. It'll appear with a copy button and everything, and end in .git
```
git remote add origin remote_repository_URL_here
```

Verify the new remote URL
```
git remote -v
```

Set upstream and then push only the lfs files to remote
```
git lfs push origin master
```

Afterwards push normally to upload everything
```
git push --set-upstream origin master
```

You only need to write --set-upstream origin master the first time for normal `push`, after this just write push. For git lfs you always have to write it.

### Check your branches in git log history in a pretty line

This makes your history tree pretty and easy to understand inside of the terminal.
I found this in https://stackoverflow.com/a/35075021

```
git log --all --decorate --oneline --graph
```

Not everyone would be doing a git log all the time, but when you need it just remember: 
"A Dog" = git log --all --decorate --oneline --graph

Actually, let's set an alias:

```
git config --global alias.adog "log --all --decorate --oneline --graph"
```

This adds the following to the .gitconfig file:

```
[alias]
        adog = log --all --decorate --oneline --graph
```

And you run it like:

```
git adog
```

## Install and setup Ruby, Bundler and Jekyll for websites

I also make websites on my free time, and lots of researchers have their projects on a github pages website. For this, I like to use Jekyll in combination with github pages. First lets install all our dependencies.

```
brew install ruby
```

Then, we have to add to the $PATH so that ruby gems are found:

```
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="/usr/local/opt/ruby/bin:/usr/local/lib/ruby/gems/X.X.0/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="$HOME/.gem/ruby/X.X.0/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Replace `X.X` with whichever version you installed.

```
gem install --user-install bundler jekyll
```

This should install both of them as user-install, but if errors occur just install separately instead of together.


Now it's installed! Well, to make a Jekyll Github Page I followed this tutorial, so go ahead and do it:

https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll

Now, once you have your webiste repository and you're ready to test the jekyll serve, do the following:

```
cd (your_repository_here)
bundle init
bundle add jekyll
bundle add webrick
```

And then all that's left to do is to serve the website with jekyll!

```
bundle exec jekyll serve
```

Now you can work on the website and look at how it changes on screen.

---

That is all for now. This is my initial setup for the lab environment under a proxy. If I have any projects that need further tinkering, that goes on another repository / tutorial.

