# Setting up Git

While I understand the irony of putting instructions on how to setup Git, on Git, consider that this instructional content can be reached via a web browser and without already having Git installed.

# (1) Installation

The easiest way to install Git is to install GitHub Desktop https://desktop.github.com/,

> Installing GitHub Desktop will also install the latest version of Git if you don't already have it. With GitHub Desktop, you get a command line version of Git with a robust GUI. Regardless of if you have Git installed or not, GitHub Desktop offers a simple collaboration tool for Git. 

- https://github.com/git-guides/install-git

It will handle the complexities of instaling Git while also making it available via your approprate command-line.

Otherwise there are various operating system specific methods, some of which can be done using only the command-line:

## Windows

See the [Git Installer for Windows](https://gitforwindows.org/).

Reference: https://github.com/git-guides/install-git

## Mac

Option 1: See the [Git Installer for Mac](https://sourceforge.net/projects/git-osx-installer/files/git-2.23.0-intel-universal-mavericks.dmg/download?use_mirror=autoselect)

Option 2: Install [Homebrew](https://brew.sh/), and then run `brew install git`

Reference: https://github.com/git-guides/install-git

## Linux

Install `apt` and run `sudo apt-get install git-all`

Reference: https://github.com/git-guides/install-git

# (2) Installation Verification

If you installed Github Desktop, being able to launch the app shows you that it is at least there.

This is done by running the following at whatever your command-line is:

```bash
$ git --version
git version 2.32.1 (Apple Git-133)
```

If you get outout showing a version, it worked. Otherwise it didn't and you need to go back to the drawing board with whatever you did: https://github.com/git-guides/install-git

# (3A) Github.com

> GitHub, Inc. is an Internet hosting service for software development and version control using Git. It provides the distributed version control of Git plus access control, bug tracking, software feature requests, task management, continuous integration, and wikis for every project

- http://github.com/

Specifically http://github.com/ is a place where you host, build, and test your code, using Git as the underlying source control system.

Consider though that the days of username/password authentication are gone, and Github.com won't allow you to use it without either setting up keys at the command-line, or by direclty using Github Desktop. Since the core of development and all of CI/CD is the command-line, it is going to be important to setup those keys.

The basis for this information is the following, whch gets into OS specific details:

1. Key Generation - https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
2. Adding Keys to your account: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

## 1. Generate a new key pair

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
 > Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM: ~/.ssh/github_com
```

It is highly recommended that you give it the key name of `~/.ssh/github_com`, so that it won't accidentally overwrite anything you may have already put there.

This will specifically generate two files:

- ~/.ssh/github_com - The private key
- ~/.ssh/github_com.pub - The public key, which is what you will provide to Github.com

## 2. Starting the SSH Agent

```bash
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

This will start the SSH agent if it is not already running.

## 3. Adding the new private key

### Windows/Linux

```bash
$ ssh-add ~/.ssh/github_com
```

### Mac

Mac is a bit different in that there is an potential extra step, along wth a slightly different add command.

If ~/.ssh/config does not exist, create it. You then have to add this content to it:

```
Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github_com
```

You then add the private key using this command:

```bash
$ ssh-add --apple-use-keychain ~/.ssh/github_com
```

## 5. Copying your new public key to the clipboard

The purpose here is a copy the content of your new public key, so you can paste it into Github.com.

### Mac

```bash
$ pbcopy < ~/.ssh/github_com.pub
```

### Windows

```bash
$ clip < ~/.ssh/github_com.pub
```

### Linux

```bash
$ cat ~/.ssh/github_com.pub
  # Then select and copy the contents of the github_com.pub file
  # displayed in the terminal to your clipboard
```

## 6. Adding the new public key to your account

In the upper-right corner of any page, click your profile photo, then click **Settings**.

![](https://docs.github.com/assets/cb-34573/images/help/settings/userbar-account-settings.png)

In the "Access" section of the sidebar, click **SSH and GPG keys**.

![](https://docs.github.com/assets/cb-28257/images/help/settings/ssh-add-ssh-key-with-auth.png)

In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal laptop, you might call this key "Personal laptop".

Paste your key into the "Key" field.

![](https://docs.github.com/assets/cb-47495/images/help/settings/ssh-key-paste-with-type.png)

Click **Add SSH key**.

![](https://docs.github.com/assets/cb-6592/images/help/settings/ssh-add-key.png)

## 7. Verifying it worked

If it worked, pick a location on your computer, and run the following command to clone this project via SSH:

```bash
$ git clone git@github.com:jvalentino/setup-git.git
```

If it worked, you will have a copy of this entire project now locally.

If it didn't, time to go back to the drawing board. The error messages are your friend, and can be pasted into Google.

# (3B) Bitbucket.com

TBD

# (3C) Gitlab

TBD

# (3D) AWS CodeCommit

TBD

# (3E) Azure DevOps

TBD

