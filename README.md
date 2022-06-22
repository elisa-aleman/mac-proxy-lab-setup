# Mac Python setup environment for Machine learning Laboratory on a proxy

This is how I set up a fresh mac to start working in machine learning and programming. My lab runs under a proxy, so all the settings have that one extra ...sigh... task to work correctly. I keep this tutorial handy in case I do a clean OS install or if I need to check some of my initial settings.

<!-- MarkdownTOC autolink="true" autoanchor="true" -->

- [Basic Settings](#basic-settings)
    - [Install SublimeText](#install-sublimetext)
        - [Easy TeX math: Add paired $ signs to the keybinds](#easy-tex-math-add-paired--signs-to-the-keybinds)
    - [Easily transform 2 spaced indent to 4 spaced indent](#easily-transform-2-spaced-indent-to-4-spaced-indent)
- [Setup proxy system wise](#setup-proxy-system-wise)
    - [Normal settings](#normal-settings)
    - [Time settings](#time-settings)
- [Setup proxy settings in bash](#setup-proxy-settings-in-bash)
- [Install Homebrew](#install-homebrew)
    - [Install Xcode Command Line Tools](#install-xcode-command-line-tools)
    - [Install Homebrew under a proxy](#install-homebrew-under-a-proxy)
- [Curl proxy settings](#curl-proxy-settings)
- [Install Python with pyenv](#install-python-with-pyenv)
    - [Install virtualenv](#install-virtualenv)
    - [Useful Data Science libraries](#useful-data-science-libraries)
- [Install and setup Flask for Python Web Development](#install-and-setup-flask-for-python-web-development)
- [Install and setup Git](#install-and-setup-git)
    - [Check your branches in git log history in a pretty line](#check-your-branches-in-git-log-history-in-a-pretty-line)
    - [GitHub Markdown math expressions for README.md, etc.](#github-markdown-math-expressions-for-readmemd-etc)
    - [GitLab Markdown math expressions for README.md, etc.](#gitlab-markdown-math-expressions-for-readmemd-etc)
    - [Install Git Large File System](#install-git-large-file-system)
    - [Make a new Git \(LFS\) repository from local](#make-a-new-git-lfs-repository-from-local)
    - [Manage multiple GitHub or GitLab accounts](#manage-multiple-github-or-gitlab-accounts)
- [Install and setup Ruby, Bundler and Jekyll for websites](#install-and-setup-ruby-bundler-and-jekyll-for-websites)
- [Install MacTeX and latexdiff](#install-mactex-and-latexdiff)
- [Shell Scripting for convenience](#shell-scripting-for-convenience)
    - [Basic flag setup with getopts](#basic-flag-setup-with-getopts)
    - [Argparse-bash by nhoffman](#argparse-bash-by-nhoffman)
    - [LaTeX helpers](#latex-helpers)
- [Install Pandoc to convert/export markdown, HTML, LaTeX, Word](#install-pandoc-to-convertexport-markdown-html-latex-word)
- [Accessibility Stuff](#accessibility-stuff)
    - [Accessible Color Palettes with Paletton](#accessible-color-palettes-with-paletton)
    - [Reading tools for Neurodivergent people](#reading-tools-for-neurodivergent-people)
    - [Reading white PDFs](#reading-white-pdfs)
        - [Firefox](#firefox)
        - [Microsoft Edge](#microsoft-edge)

<!-- /MarkdownTOC -->


<a id="basic-settings"></a>
## Basic Settings

Setup root password:
https://www.cyberciti.biz/faq/how-to-change-root-password-on-macos-unix-using-terminal/

```
sudo passwd root
```

- Set up the screen lock command so you can do it every time you stand up:

https://achekulaev.medium.com/lock-screen-with-cmd-l-l-global-hotkey-on-macos-high-sierra-3c596b76026a

- Set up your WiFi connection.
- For ease of use, make hidden files and file extensions visible.

Hidden files visibility:
I can't work without seeing hidden files, so in the newer versions of MacOSX we can do `CMD+Shift+.` and the hidden files will appear. 
If this doesn't work, open up the terminal and:
```
defaults write com.apple.finder AppleShowAllFiles YES
```


<a id="install-sublimetext"></a>
### Install SublimeText

- Install SublimeText4 for ease of use (this is my personal favorite, but it's not necessary)

https://www.sublimetext.com/download

- Paste the SublimeText4 preferences (my personal preferences)

```
{
    "ignored_packages":
    [
        "Vintage",
    ],
    "spell_check": true,
    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    "copy_with_empty_selection": false
}
```

Also, Sublime Text is all about the plugins. Install Package Control by typing CTRL+Shift+P, then typing "Install Package Control"

Then here's some cool packages to try:

- [LaTeXTools](https://packagecontrol.io/packages/LaTeXTools)
- [MarkdownTOC](https://packagecontrol.io/packages/MarkdownTOC)
- [MarkdownPreview](https://packagecontrol.io/packages/MarkdownPreview)
- [IncrementSelection](https://packagecontrol.io/packages/Increment%20Selection)

In MarkdownTOC.sublime-settings, paste the following for hyperlink markdowns and compatibility with MarkdownPreview:

```
{
  "defaults": {
    "autoanchor": true,
    "autolink": true,
    "markdown_preview": "github"
  },
}
```

<a id="easy-tex-math-add-paired--signs-to-the-keybinds"></a>
#### Easy TeX math: Add paired $ signs to the keybinds

I found myself needing paired $dollar signs$ for math expressions in either LaTeX, GitLab with KeTeX or GitHub also with a different syntax but still some interpretation of TeX.

Searching for [how to do it on macros](https://forum.sublimetext.com/t/snippet-wrap-current-line-or-selection/53285), I found this post about keybindings which is a way better solution:

https://stackoverflow.com/questions/34115090/sublime-text-2-trying-to-escape-the-dollar-sign

Which, as long as we implement the double escaped dollar sign solution, we can use freely.

- Preferences > Key Bindings:
- Add this inside the brackets:

```
// Auto-pair dollar signs
{ "keys": ["$"], "command": "insert_snippet", "args": {"contents": "\\$$0\\$"}, "context":
    [
        { "key": "setting.auto_match_enabled", "operator": "equal", "operand": true },
        { "key": "selection_empty", "operator": "equal", "operand": true, "match_all": true },
        { "key": "following_text", "operator": "regex_contains", "operand": "^(?:\t| |\\)|]|\\}|>|$)", "match_all": true },
        { "key": "preceding_text", "operator": "not_regex_contains", "operand": "[\\$a-zA-Z0-9_]$", "match_all": true },
        { "key": "eol_selector", "operator": "not_equal", "operand": "string.quoted.double", "match_all": true }
    ]
},
{ "keys": ["$"], "command": "insert_snippet", "args": {"contents": "\\$${0:$SELECTION}\\$"}, "context":
    [
        { "key": "setting.auto_match_enabled", "operator": "equal", "operand": true },
        { "key": "selection_empty", "operator": "equal", "operand": false, "match_all": true }
    ]
},
{ "keys": ["$"], "command": "move", "args": {"by": "characters", "forward": true}, "context":
    [
        { "key": "setting.auto_match_enabled", "operator": "equal", "operand": true },
        { "key": "selection_empty", "operator": "equal", "operand": true, "match_all": true },
        { "key": "following_text", "operator": "regex_contains", "operand": "^\\$", "match_all": true }
    ]
},
{ "keys": ["backspace"], "command": "run_macro_file", "args": {"file": "Packages/Default/Delete Left Right.sublime-macro"}, "context":
    [
        { "key": "setting.auto_match_enabled", "operator": "equal", "operand": true },
        { "key": "selection_empty", "operator": "equal", "operand": true, "match_all": true },
        { "key": "preceding_text", "operator": "regex_contains", "operand": "\\$$", "match_all": true },
        { "key": "following_text", "operator": "regex_contains", "operand": "^\\$", "match_all": true }
    ]
},
```

<a id="easily-transform-2-spaced-indent-to-4-spaced-indent"></a>
### Easily transform 2 spaced indent to 4 spaced indent

https://forum.sublimetext.com/t/can-i-easily-change-all-existing-2-space-indents-to-4-space-indents/40158/2

- Sublime text, lower right corner
- Click on Spaces
- Select the current space number
- Click Convert indentation to Tabs
- Select the desired space number
- Click Convert indentation to Spaces

<a id="setup-proxy-system-wise"></a>
## Setup proxy system wise

<a id="normal-settings"></a>
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

<a id="time-settings"></a>
### Time settings

Some proxies need you to set up the time of the computer to match the network in order to work correctly. Mine does at least. So the settings are such:

Settings > Date & Time > Date & Time

Set Date and Time automatically:
{TIME_URL}

The {TIME_URL} will depend on your organization, so check with your administrator.

<a id="setup-proxy-settings-in-bash"></a>
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

<a id="install-homebrew"></a>
## Install Homebrew

For more info, click [here](https://brew.sh).

First we need to consider the macOS Requirements from their website:

- A 64-bit Intel CPU
- macOS High Sierra (10.13) (or higher)
- Command Line Tools (CLT) for Xcode: `xcode-select --install`, [developer.apple.com/downloads](https://developer.apple.com/downloads) or [Xcode](https://itunes.apple.com/us/app/xcode/id497799835)
- A Bourne-compatible shell for installation (e.g. bash or zsh)

As it says in the third requirement, we need the Command Line Tools for Xcode.

<a id="install-xcode-command-line-tools"></a>
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

<a id="install-homebrew-under-a-proxy"></a>
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

<a id="curl-proxy-settings"></a>
## Curl proxy settings

Right there installing Homebrew we used explicit proxy settings on the curl command to avoid any issues, but to avoid doing this every time for future uses of curl, we also need to setup proxy settings.

```
sudo nano ~/.curlrc
```

And add the following:
```
proxy = {PROXY_HOST}:{PORT} 
```

<a id="install-python-with-pyenv"></a>
## Install Python with pyenv

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

<a id="install-virtualenv"></a>
### Install virtualenv

Virtualenv allows me to install different libraries under pip for specific projects.

```
pip install virtualenv
```

Usage guide is here:<br>
https://mothergeo-py.readthedocs.io/en/latest/development/how-to/venv-win.html

<a id="useful-data-science-libraries"></a>
### Useful Data Science libraries

This is my generic fresh start install so I can work. There's more libraries with complicated installations in other repositories of mine, and you might not wanna run this particular piece of code without checking what I'm doing first. For example, you might have a specific version of Tensorflow that you want, or some of these you won't use. But I'll leave it here as reference.

```
pip install numpy scipy statsmodels matplotlib \
sklearn beautifulsoup4 requests selenium gensim \
nltk langdetect sympy pyclustering \
tensorflow tflearn keras minepy
```

Although some of these (specifically minepy) need the Visual Studio C++ Build Tools as a dependency, so install it first:<br>
https://visualstudio.microsoft.com/visual-cpp-build-tools/

```
pip install minepy
```

And some more python libraries that will be useful for regular tasks:

```
pip install more_itertools pandas pathlib tqdm retry
```

<a id="install-and-setup-flask-for-python-web-development"></a>
## Install and setup Flask for Python Web Development

I'm using this guide:
https://flask.palletsprojects.com/en/2.1.x/installation/#install-flask

For Web Development, it's apparently better to make a Virtual Environment to install flask project-wise instead of system or user level.

First we go to our project and make the virtual environment.

```
cd my-project
python -m venv venv
. venv/scripts/activate
```
it might also be
```
. venv/bin/activate
```
so if one fails try the other.

Activate results in the python and pip versions to be internal to the project now and active in the bash:

```
which python
>>> .... my-project\venv/Scripts/python

pip --version
pip 22.1 from c:\users\...\my-project\venv\lib\site-packages\pip (python 3.9)
```

So since this pip is empty, let's install ONLY what we need for the project:

```
pip install --upgrade pip
pip install selenium beautifulsoup4 pandas pathlib retry
pip install flask
```

Now let's test it:

I'm using this guide here:

https://flask.palletsprojects.com/en/2.1.x/quickstart/

Under: `my-project/python/hello.py`
```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```
Then in bash:

```
cd my-project/python
export FLASK_APP=hello
flask run
```
and it should return:

```
 * Serving Flask app 'hello' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
```

Unlike Jekyll, it won't update as you edit, unless you set up a development environment variable:

```
export FLASK_APP=hello
export FLASK_ENV=development
flask run
```

It will update, but you still have to click refresh on the screen.

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<h1>Hello, World!</h1>"
```

Now to make the project a package and keep running under the same structure as before, now I use this structure: (which by the way you can output with `tree /F /A | clip` on the CMD)

```
my-project
|   .gitignore
|   README.md
|   requirements.txt
|   
+---logs
|       tasklog.md
|       
+---python
|   |   ProjectPaths.py
|   |   
|   +---package
|   |   |   module.py
|   |   |   __init__.py
|   |   |   
|   |   \---__pycache__
|   |           
|   +---tests
|   |   |   0_test.py
|   |   |   
|   |   \---__pycache__
|   |           
|   \---__pycache__
|           
\---venv
```

`ProjectPaths.py` is just a bunch of methods I like to call to make directories without much hassle.

To run the tests and the files in the package, now the command is like this:

```
cd my-project/python
python -m tests.0_test
```

Notice that it's a `.` instead of a `/` and also there's no `.py`
It should now run and import as if from the parent directory `python`

<a id="install-and-setup-git"></a>
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
git config --global color.ui auto
git config --global merge.conflictstyle diff3
git config --global core.editor nano
git config --global core.autocrlf input
git config --global core.fileMode false
git config --global pull.ff only
```

This should make a file `~/.gitconfig` with the following text
```
# ~/.gitconfig

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
    fileMode = false
[pull]
    ff = only
[alias]
    adog = log --all --decorate --oneline --graph
```

That last one, `git adog` is very useful as I explain in [Check your branches in git log history in a pretty line](#check-your-branches-in-git-log-history-in-a-pretty-line)

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

<a id="check-your-branches-in-git-log-history-in-a-pretty-line"></a>
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

<a id="github-markdown-math-expressions-for-readmemd-etc"></a>
### GitHub Markdown math expressions for README.md, etc.

Following this guide, math is different in GitLab markdown than say, GitHub or LaTeX.
However, inside of the delimiters, it renders it using KaTeX, which uses LaTeX math syntax! 

https://docs.gitlab.com/ee/user/markdown.html#math

Inline: 
```
> $a^2 + b^2 = c^2$
```

Renders as: $a^2 + b^2 = c^2$

Block:
```
> $$a^2 + b^2 = c^2$$
```

Renders as:

$$a^2 + b^2 = c^2$$

But it only supports one line of math, so for multiple lines you have to do this:

```
> $$a^2 + b^2 = c^2$$
> <!-- (line break is important) -->
> $$c = \sqrt{ a^2 + b^2 }$$
```

Renders as:

$$a^2 + b^2 = c^2$$

$$c = \sqrt{ a^2 + b^2 }$$

It can even display matrices and the like:

```
> $$
> l_1 = 
> \begin{bmatrix}
>     \begin{bmatrix}
>         x_1 & y_1
>     \end{bmatrix} \\
>     \begin{bmatrix}
>         x_2 & y_2
>     \end{bmatrix} \\
>     ... \\
>     \begin{bmatrix}
>         x_n & y_n
>     \end{bmatrix} \\
> \end{bmatrix}
> $$
```

$$
l_1 = 
\begin{bmatrix}
    \begin{bmatrix}
        x_1 & y_1
    \end{bmatrix} \\
    \begin{bmatrix}
        x_2 & y_2
    \end{bmatrix} \\
    ... \\
    \begin{bmatrix}
        x_n & y_n
    \end{bmatrix} \\
\end{bmatrix}
$$


However, % comments will break the environment.

Math syntax in LaTeX:

https://katex.org/docs/supported.html

<a id="gitlab-markdown-math-expressions-for-readmemd-etc"></a>
### GitLab Markdown math expressions for README.md, etc.

Following this guide, math is different in GitLab markdown than say, GitHub or LaTeX.
However, inside of the delimiters, it renders it using KaTeX, which uses LaTeX math syntax! 

https://docs.gitlab.com/ee/user/markdown.html#math

Inline: 
```
> $`a^2 + b^2 = c^2`$
```

Renders as: $`a^2 + b^2 = c^2`$

Block:
```
> ```math
> a^2 + b^2 = c^2
> ```
```

Renders as:

```math
a^2 + b^2 = c^2
```

But it only supports one line of math, so for multiple lines you have to do this:

```
> ```math
> a^2 + b^2 = c^2
> ```
> ```math
> c = \sqrt{ a^2 + b^2 }
> ```
```

Renders as:

```math
a^2 + b^2 = c^2
```
```math
c = \sqrt{ a^2 + b^2 }
```

It can even display matrices and the like:

```
> ```math
> l_1 = 
> \begin{bmatrix}
>     \begin{bmatrix}
>         x_1 & y_1
>     \end{bmatrix} \\
>     \begin{bmatrix}
>         x_2 & y_2
>     \end{bmatrix} \\
>     ... \\
>     \begin{bmatrix}
>         x_n & y_n
>     \end{bmatrix} \\
> \end{bmatrix}
> ```
```

```math
l_1 = 
\begin{bmatrix}
    \begin{bmatrix}
        x_1 & y_1
    \end{bmatrix} \\
    \begin{bmatrix}
        x_2 & y_2
    \end{bmatrix} \\
    ... \\
    \begin{bmatrix}
        x_n & y_n
    \end{bmatrix} \\
\end{bmatrix}
```

However, % comments will break the environment.

Math syntax in LaTeX:

https://katex.org/docs/supported.html

<a id="install-git-large-file-system"></a>
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

<a id="make-a-new-git-lfs-repository-from-local"></a>
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

<a id="manage-multiple-github-or-gitlab-accounts"></a>
### Manage multiple GitHub or GitLab accounts

Because I want to update my personal code when I find better ways to program at work, I want to push and pull from my personal GitHub account aside from the work GitLab projects. **CAUTION: DON'T UPLOAD COMPANY SECRETS TO YOUR PERSONAL ACCOUNT**

To be able to do this, I followed these guides:<br>
https://blog.gitguardian.com/8-easy-steps-to-set-up-multiple-git-accounts/


https://medium.com/the-andela-way/a-practical-guide-to-managing-multiple-github-accounts-8e7970c8fd46


1. Generate an SSH key
First, create an SSH key for your personal account:
```
ssh-keygen -t rsa -b 4096 -C "your_personal_email@example.com" -f ~/.ssh/<personal_key> 
```
Then for your work account:
```
ssh-keygen -t rsa -b 4096 -C "your_work_email@company.com" -f ~/.ssh/<work_key> 
```

2. Add a passphrase

Then add a passphrase and press enter, it will ask for it twice. Press enter again.

To update the passphrase for your SSH keys:

```
ssh-keygen -p -f ~/.ssh/<personal_key>
```

You can check your newly created key with:

```
ls -la ~/.ssh
```

which should output <personal_key> and <personal_key>.pub.

Do the same steps for the <work_key>.

3. Tell ssh-agent

The website has an -K tag that works for macOSX and such but we don't need it.

```
eval "$(ssh-agent -s)" && \
ssh-add ~/.ssh/<personal_key> 
ssh-add ~/.ssh/<work_key> 
```

4. Edit your SSH config

```
nano ~/.ssh/config
-----------nano----------
# Work account - default
Host <some_host_name_work>
  HostName <HOST>:<PORT>
  User git
  IdentityFile ~/.ssh/<work_key> 

# Personal account
Host <personal_host_name>
  HostName github.com
  User git
  IdentityFile ~/.ssh/<personal_key>

CTRL+O
CTRL+X
-------------------------
```

5. Copy the SSH public key

```
cat ~/.ssh/<personal_key>.pub | clip
```

Then paste on your respective website settings, such as the GitHub SSH settings page. Title it something you'll know it's your work computer.

Same for your <work_key>

6. Structure your workspace for different profiles

Now, for each key pair (aka profile or account), we will create a .conf file to make sure that your individual repositories have the user settings overridden accordingly.
Let’s suppose your home directory is like that:

```
/myhome/
    |__.gitconfig
    |__work/
    |__personal/
```

We are going to create two overriding .gitconfigs for each dir like this:

```
/myhome/
|__.gitconfig
|__work/
     |_.gitconfig.work
|__personal/
    |_.gitconfig.pers
```

Of course the folder and filenames can be whatever you prefer.

7. Set up your Git configs

In the personal git projects folder, make `.gitconfig.pers`

```
nano ~/personal/.gitconfig.pers
---------------nano-----------------
# ~/personal/.gitconfig.pers
 
[user]
email = your_personal_email@example.com
name = Your Name
 
[github] #or gitlab or whatever
user = "personal-username"
 
[core]
sshCommand = “ssh -i ~/.ssh/<personal_key>”

```


```

# ~/work/.gitconfig.work
 
[user]
email = your_work_email@company.com
name = Your Name
 
[github] #or gitlab or whatever
user = "work_username"

[core]
sshCommand = “ssh -i ~/.ssh/<work_key>”


```

And finally add this to the end of your original main `.gitconfig` file:

```
[includeIf “gitdir:~/personal/”] # include for all .git projects under personal/ 
path = ~/personal/.gitconfig.pers
 
[includeIf “gitdir:~/work/”]
path = ~/work/.gitconfig.work
```

Now finally to confirm if it worked, go to any work project you have and type the following:

```
cd ~/work/work-project
git config user.email
```

It should be your work e-mail.

Now go to a personal project:

```
cd ~/personal/personal-project
git config user.email
```

And it should output your personal e-mail.

To clone new projects, specially private or protected ones, use the username before the website:

```
git clone https://<username>@github.com/<organization>/<repo>.git
```

And done! When you push or pull from the personal account you might encounter some 2 factor authorizations at login, but otherwise it's ready to work on both personal and work projects.

<a id="install-and-setup-ruby-bundler-and-jekyll-for-websites"></a>
## Install and setup Ruby, Bundler and Jekyll for websites

I also make websites on my free time, and lots of researchers have their projects on a github pages website. For this, I like to use Jekyll in combination with github pages. First lets install all our dependencies.

```
brew install ruby
```

Then, we have to add to the $PATH so that ruby gems are found:

```
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="~/.local/share/gem/ruby/X.X.X/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Where `X.X.X ` is the version you have installed.

In my case it was:

```
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zprofile
echo 'export PATH="~/.local/share/gem/ruby/3.0.0/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Check that we're indeed using the Homebrew version of ruby:

```
which ruby
```
Which should output this:
```
/usr/local/opt/ruby/bin/ruby
```

```
gem install --user-install bundler jekyll jekyll-sitemap
```

This should install both of them as user-install, but if errors occur just install separately instead of together.


Now it's installed! Well, to make a Jekyll Github Page I followed this tutorial, so go ahead and do it:

https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll

Now, once you have your webiste repository and you're ready to test the jekyll serve, do the following:

```
cd (your_repository_here)
bundle init
bundle add jekyll
bundle add jekyll-sitemap
bundle add webrick
```

And then all that's left to do is to serve the website with jekyll!
Also for the sitemaps make sure to check this tutorial:

https://github.com/jekyll/jekyll-sitemap

And add this to your `_config.yml`
```
url: "https://example.com" # the base hostname & protocol for your site
plugins:
  - jekyll-sitemap
```

```
bundle exec jekyll serve
```

If you get an error like:
```
Could not find webrick-1.7.0 in any of the sources
Run `bundle install` to install missing gems.
```

Do as it says and just run:
```
bundle install
```

Now you can work on the website and look at how it changes on screen.

By the way, if you are hosting on GitHub Pages and have a custom domain, you need to add these to the DNS

```
Type    Name    Points to               TTL
a       @       185.199.108.153         600 seconds
a       @       185.199.109.153         600 seconds
a       @       185.199.110.153         600 seconds
a       @       185.199.111.153         600 seconds
cname   www     your-username.github.io 600 seconds   
```

<a id="install-mactex-and-latexdiff"></a>
## Install MacTeX and latexdiff

Download the `.pkg` file and install it manually.

https://www.tug.org/mactex/downloading.html

This installs a few packages along with it, including latexdiff which I use a lot as a PhD student.
It is installed under `/Library/TeX/texbin`, but if you have any terminal sessions open it will probably not load, so you should open a new session, and then confirm that it also installed latexdiff with:
```
which latexdiff
```

For using these I've made a few helper files, which can be seen here:
https://github.com/elisa-aleman/latex_helpers

Also Install the SublimeText LaTeXTools package.


<a id="shell-scripting-for-convenience"></a>
## Shell Scripting for convenience

When it comes down to it, specially when working with LaTeX or git, you find yourself making the same commands over and over again. That takes time and frustration, so I find that making scripts from time to time saves me a lot of time in the future.


<a id="basic-flag-setup-with-getopts"></a>
### Basic flag setup with getopts

Once in a while those scripts will need some input to be more useful in as many cases as possible instead of a one time thing.

Looking for how to do this I ran across [a simple StackOverflow question](https://stackoverflow.com/questions/14447406/bash-shell-script-check-for-a-flag-and-grab-its-value), which led me to the `getopts` package and its tutorial:

[Getopts manual](https://archive.ph/TRzn4)

This is a working example:

```
while getopts ":a:" opt; do
  case $opt in
    a)
      echo "-a was triggered, Parameter: $OPTARG" >&2
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      exit 1
      ;;
    :)
      echo "Option -$OPTARG requires an argument." >&2
      exit 1
      ;;
  esac
done

```

<a id="argparse-bash-by-nhoffman"></a>
### Argparse-bash by nhoffman

Now sometimes you'll want to have fancy arguments with both a shortcut name (-) and a long name (--), for example `-a` and `--doall` both pointing to the same command. In that case I recommend using nhoffman's implementation of Python's `argparse` in bash:

[argparse.bash by nhoffman on GitHub](https://github.com/nhoffman/argparse-bash)

<a id="latex-helpers"></a>
### LaTeX helpers

Personally, I find it tiring to try to compile a LaTeX document, only to have to run the bibliography, and then compile the document twice again so all the references are well put where they need to be, rather tiring. Also, I find that the output files are cluttering my space and I only need to see them when I run into certain errors.

Also, for academic papers, I used `latexdiff` commands quite a lot, and while customizable, I noticed I needed a certain configuration for most journals and that was it.

So I made [LaTeX helpers](https://github.com/elisa-aleman/latex_helpers), a couple of bash scripts that make that process faster.

So instead of typing

```
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex
open paper.tex
rm paper.log paper.out paper.aux paper.... and so on
```

Every. Single. Time. 

I just need to type:
```
./latexcompile.sh paper.tex --view --clean
```

and if I needed to make a latexdiff I just:

```
./my_latexdiff.sh paper_V1-1.tex paper.tex --newversion="2" --compile --view --clean
```

And there it is, a latexdiff PDF right on my screen.

I would also commonly have several documents of different languages, or save my latexdiff command in another script, called `cur_compile_all.sh` or `cur_latexdiff.sh` so I didn't have to remember version numbers and stuff when working across several weeks or months.

Usually with code such as:

```
cd en
./latexcompile.sh paper.tex --view --clean --xelatex
cd ../es
./latexcompile.sh paper.tex --view --clean --xelatex
cd ../jp
./latexcompile.sh paper.tex --view --clean --xelatex
```

And so on, to save time.

<a id="install-pandoc-to-convertexport-markdown-html-latex-word"></a>
## Install Pandoc to convert/export markdown, HTML, LaTeX, Word

I discovered this tool recently when I was asked to share a PDF of my private GitLab MarkDown notes. Of course I wouldn't share the whole repository so that it can be displayed in GitLab for them, so I searched for an alternative. 

It can be installed in Windows, macOS, Linux, ChromeOS, BSD, Docker, ... it's really portable

Pandoc Install:  
https://pandoc.org/installing.html


Pandoc Manual:  
https://pandoc.org/MANUAL.html


Export to PDF syntax
```
pandoc test1.md -s -o test1.pdf
```

Note that it uses LaTeX to convert to PDF, so UTF-8 languages (japanese, etc.) might return errors.

```
pandoc test1.md -s -o test1.pdf --pdf-engine=xelatex
```

But it doesn't load the Font for Japanese... Also, the default margins are way too wide.

So, in the original markdown file preamble we need to add [Variables for LaTeX](https://pandoc.org/MANUAL.html#variables-for-latex):

```
---
title: "Title"
author: "Name"
date: YYYY-MM-DD
<!-- add the following -->
geometry: margin=1.5cm
output: pdf_document
CJKmainfont: IPAMincho
---
```

And voilà, the markdown is now a PDF.

I'm still unsure if it will process the GitHub or GitLab math environments, since the syntax is different.

Upon confirmation with the [User's Guide: Math](https://pandoc.org/MANUAL.html#math) section, it uses the GitHub math syntax.

Inline: `$x=3$`  
Renders as:  
$x=3$


Block:  `$$x=3$$`  
Renders as:  
$$x=3$$


<a id="accessibility-stuff"></a>
## Accessibility Stuff

<a id="accessible-color-palettes-with-paletton"></a>
### Accessible Color Palettes with Paletton 

When designing new things it's important to keep in mind color theory, as well as accessibility for the visually impaired and color blind people, etc. But that's so much time one could spend doing so much else, so here's a tool that can help with that and also visualizing how other people with different ranges of color vision would perceive it. It's called Paletton.

https://paletton.com

<a id="reading-tools-for-neurodivergent-people"></a>
### Reading tools for Neurodivergent people

There was a new tool developed called "Bionic Reading", which bolds the beginnings of words so that our eyes glide over them more easily, basically making a tool for speed reading without having to train specifically for that. Lots of neurodivergent people such as myself (I have ADHD and am autistic), have a hard time following long texts or focusing when there is too much information at the same time (say, with very small line spacing). This new tool has been praised by the ND (neurodivergent) community, since making it available for businesses or companies to use would mean more accessibility in everyday services...  or at least it was until they decided to charge an OUTRAGEOUS amount of money to implement it, making it obviously not attractive for companies to implement and therefore ruining it for everyone.

That is why someone decided to make "Not Bionic Reading" which is, legally speaking, not the same thing as Bionic Reading and therefore can be made available for everyone as Open Source.

Here's the usable link:
https://not-br.neocities.org/

Have fun reading!

<a id="reading-white-pdfs"></a>
### Reading white PDFs

<a id="firefox"></a>
#### Firefox

https://pncnmnp.github.io/blogs/firefox-dark-mode.html

> After hunting on the web for about 30 minutes, I found this thread on Bugzilla. It turns out starting with Firefox 60, extensions are no longer allowed to interact with the native pdf viewer. Determined, I decided to locally modify the CSS rendered by Firefox's PDF viewer. The steps for the same are:
>
> - Open Firefox and press Alt to show the top menu, then click on Help → Troubleshooting Information
> - Click the Open Directory button beside the Profile Directory entry
> - Create a folder named chrome in the directory that opens
> - In the chrome folder, create a CSS file with the name userContent.css
> - Open the userContent.css file and insert -

```
#viewerContainer > #viewer > .page > .canvasWrapper > canvas {
    filter: grayscale(100%);
    filter: invert(100%);
}
```
s
> - On Firefox's URL bar, type about:config.
> - Search for toolkit.legacyUserProfileCustomizations.stylesheets and set it to true.
> - Restart Firefox and fire up a PDF file to see the change!

<a id="microsoft-edge"></a>
#### Microsoft Edge

If you ever need to read a PDF on the Microsoft Edge browser, you can create a snippet to execute on the Dev Console, as per this post.

https://www.reddit.com/r/edge/comments/nhnflv/comment/hgejdwz/?utm_source=share&utm_medium=web2x&context=3

> So, I've wanted this feature pretty badly as well and I've found a workaround which doesn't involve inverting the whole OS but still takes some extra steps:
>
> - Open the PDF you want to read
> - Right click > Inspect Element
> - Select Console tab
> - Paste the code given below
> - Hit enter
> - Profit!

```
let backgroundColor = PDFViewer.EDGE_PDFVIEWER_BACKGROUND_COLOR_LIGHT;
viewer.plugin_.setAttribute('background-color', backgroundColor);
viewer.pluginController_.postMessage({
    type: 'backgroundColorChanged',
    backgroundColor
});
document.getElementById('document-container').style.filter = 'invert()';
document.getElementById('layout-container').style.filter = 'invert()';
```

> You can utilize the snippets feature in DevTools to save the above code. To do that, do:
> 
> - Hit F12 or Ctrl + Shift + I to open DevTools
> - Once the DevTools is open, press Ctrl + Shift + P and type "new snippet" and choose the first option
> - Paste the above code
> - Right click "Script snippet #2" > Rename > "dark mode pdf"
> - Hit enter to rename
> - Close DevTools
> 
> If you did the above to save that script, the next time, you can perform the following steps to activate dark mode:
> 
> - Open PDF
> - Right click > Inspect Element
> - Press Ctrl + P
> - Type exclamation "!"
> - Hit enter (or select the snippet if you have multiple and press enter)


---

That is all for now. This is my initial setup for the lab environment under a proxy. If I have any projects that need further tinkering, that goes on another repository / tutorial.

