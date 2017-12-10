# Mac Development Ansible Playbook

[![Build Status](https://travis-ci.org/geerlingguy/mac-dev-playbook.svg?branch=master)](https://travis-ci.org/geerlingguy/mac-dev-playbook)

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.

This is a work in progress, and is mostly a means for me to document my current Mac's setup. I'll be evolving this set of playbooks over time.

*See also*:

  - [Boxen](https://github.com/boxen)
  - [Battleschool](http://spencer.gibb.us/blog/2014/02/03/introducing-battleschool)
  - [osxc](https://github.com/osxc)
  - [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks) (the original inspiration for this project)

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. [Install Ansible](http://docs.ansible.com/intro_installation.html).  `sudo pip install ansible` or `(curl -O https://bootstrap.pypa.io/get-pip.py; sudo python get-pip.py; sudo pip install ansible)`
  3. Clone this repository to your local drive.
  4. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  5. Run `ansible-playbook main.yml -i inventory -K` inside this directory. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - cowsay
      - git
      - go

    mas_installed_apps:
      - { id: 443987910, name: "1Password" }
      - { id: 498486288, name: "Quick Resizer" }
      - { id: 557168941, name: "Tweetbot" }
      - { id: 497799835, name: "Xcode" }

    composer_packages:
      - name: hirak/prestissimo
      - name: drush/drush
        version: '^8.1'

    gem_packages:
      - name: bundler
        state: latest

    npm_packages:
      - name: webpack

    pip_packages:
      - name: mkdocs

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask) TODO: Update this list:

  - [cheatsheet] (https://www.mediaatelier.com/CheatSheet/)
  - [coconutbattery] (www.coconut-flavour.com)
  - [Docker](https://www.docker.com/)
  - [Dropbox](https://www.dropbox.com/)
  - [filezilla] (https://filezilla-project.org/)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [google-backup-and-sync] (https://www.google.com/drive/download/backup-and-sync/)
  - [Homebrew](http://brew.sh/)
  - [inkscape] (https://inkscape.org/)
  - [itsycal] (https://www.mowglii.com/itsycal/)
  - [keepassx] (https://www.keepassx.org/)
  - [keka] (www.kekaosx.com/)
  - [teamviewer] (https://www.teamviewer.us/)
  - [Vagrant](https://www.vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  - [virtualbox-extension-pack] (https://www.virtualbox.org/wiki/Downloads)
  - [vlc] (https://www.videolan.org/vlc/index.html)

Packages (installed with Homebrew) TODO: Update this list:

  - bash-completion
  - git
  - go
  - hub
  - httpie
  - iperf
  - monit
  - nmap
  - ssh-copy-id
  - cowsay
  - openssl
  - pv
  - wget
  - wrk

My [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions

### TODO:

  - [ ] Clean up code and update documentation
  - [ ] Tweak OSX config
  - [ ] Split installed software into different files (core/suggested/personal)
  - [x] Add [Mackup](https://github.com/lra/mackup) information
  - [ ] Add suggested Atom/PHPStorm config/plugins
  - [ ] Add suggested git config

### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).

  1. Install [Sublime Package Manager](http://sublime.wbond.net/installation).
  2. Install all the apps that aren't yet in this setup (see below).
  3. Remap Caps Lock to Escape (requires macOS Sierra 10.12.1+).
  4. Set trackpad tracking rate.
  5. Set mouse tracking rate.
  6. Configure extra Mail and/or Calendar accounts (e.g. Google, Exchange, etc.).

### Applications/packages to be added:

These are mostly direct download links, some are more difficult to install because of custom installers or other nonstandard install quirks:

  - Microsoft Office Suite
  - OneNote

## Testing the Playbook

Many people have asked me if I often wipe my entire workstation and start from scratch just to test changes to the playbook. Nope! Instead, Jeff Geerling posted instructions for how  build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which I can continually run and re-run this playbook to test changes and make sure things work correctly.

Additionally, this project is [continuously tested on Travis CI's macOS infrastructure](https://travis-ci.org/geerlingguy/mac-dev-playbook).

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## New to Ansible?
https://github.com/blacksaildivision/ansible-tutorial

## Author
Red Airship, 2017 (inspired by [Jeff Geerling](http://www.jeffgeerling.com/))
[Jeff Geerling](http://www.jeffgeerling.com/), 2014 (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).

## Dev information
### Mackup
If you have [Dropbox](https://www.dropbox.com) installed and want to use it to
save your config files, that's super easy.

On OS X, if you want an easy install, you can install
[Homebrew](http://brew.sh/) and do:

```bash
# Install Mackup
brew install mackup

# Launch it and back up your files
mackup backup
```

If not running OS X, or you don't like Homebrew, you can use [pip](https://pip.pypa.io/en/stable/).

> Note: The below command will check if a previous version of Mackup
> is already installed on your system.
> If this is the case, it will be upgraded to the latest version.

```bash
# Install Mackup with PIP
pip install --upgrade mackup

# Launch it and back up your files
mackup backup
```

You're all set and constantly backed up from now on.

Next, on any new workstation, do:

```bash
# Install Mackup
brew install mackup

# Launch it and restore your files
mackup restore
```

Done!
### GIT

`git` by default doesn't have autocompletion on OS X.
Super easy to [install it](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion):

```bash
brew install bash-completion
```

Then add this line to your `~/.bash_profile`:

```bash
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi
```

[RECOMMENDED] Use rebase for git merge, use this to set as global config:
```bash
git config --global pull.rebase true
```

Make sure your git pushes only the current branch.
Run the following:

```bash
git config --global push.default simple
```

To have git user the OS X Keychain, run this command:

```bash
git config --global credential.helper osxkeychain
```

## References

  - https://github.com/jonathanong/osx-webdev-setup
  - https://github.com/donaldaverill/mac-dev-playbook/blob/master/config_template.yml
  - https://github.com/taniarascia/mac
  - https://gist.github.com/t-io/8255711
  - https://blog.vandenbrand.org/2016/01/04/how-to-automate-your-mac-os-x-setup-with-ansible/
  - https://github.com/galliangg/macos-setup/blob/master/playbook.yml
  - https://github.com/JamesOBenson/mac-dev-playbook/blob/master/default.config.yml
  - https://github.com/ricbra/mac-dev-playbook/blob/master/vars/main.yml
