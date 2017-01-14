# Laptop

Laptop is a playbook to set up an OS X laptop (for web/mobile development) also included some of my design tools and collaboration tools.

It installs and configures most of the software vladzz uses on his Mac for software development.

It can be run multiple times on the same machine safely. It installs, upgrades, or skips packages based on what is already installed on the machine.


## Requirements

We've tested it on;

* OS X Sierra (~10.12.2)


## Installation

### Fast Install

If you'd like to start with my default list of tools and apps (see Included Apps/Config below), then simply install with;

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/vladzz/laptop/master/install.sh)"


You can always customize the install after-the-fact (see below), and re-run the playbook. It will skip over any installed apps.

### Custom Install

If you want to add/remove to the list of apps/utils installed, its pretty straightforward.

As above, download and bootstrap the script. But stop it before it starts ansible, and edit the playbook as desired, before re-running ansible.

1. Grab and start the bootstrap script. Let it install the prereqs and clone the full `vladzz/laptop` repo locally...

      sh -c "$(curl -fsSL https://raw.githubusercontent.com/vladzz/laptop/master/install.sh)"


1. Stop the script (Ctrl+C) when ansible asks for the a 'sudo' password.

        ```
        Changing to laptop repo dir ...

        Running ansible playbook ...
        SUDO password:  ^c

        ```

1. Change into the cloned repo dir

        cd laptop

1. Edit playbook.yml and add/remove the apps/utils you want.

        vi playbook.yml

1. Kick off ansible manually

        ansible-galaxy install -r requirements.yml
        ansible-playbook playbook.yml -i hosts --ask-sudo-pass -vvvv

You can do this as many times as you like and re-run the `ansible-playbook` command. Ansible is smart enough to skip installed apps, so subsequent runs are super fast.


## Included Applications / Configuration

### Applications

Apps installed with Homebrew Cask:
  - apptrap # remove associated prefs when uninstalling
  - appcode
  - appzapper # uninstaller
  - atom # sublime without annoying popup | https://atom.io/download/mac
  - bettertouchtool # window snapping. (maybe Moom is more lightweight?)
  - cakebrew
  - calibre # berks
  - cyberduck # ftp, s3, openstack
  - dash # totally sick API browser
  - diffmerge # free visual diq
  - disk-inventory-x # reclaim space on your expensive-ass Apple SSD | http://www.derlien.com/
  - docker-toolbox
  - docker
  - flux # get more sleep
  - google-chrome
  - google-drive
  - hipchat
  - iterm2
  - iconjar
  - icons8
  - mounty
  - qlcolorcode # quick look syntax highlighting
  - qlimagesize # quick look image dimensions
  - qlmarkdown # quick look .md files
  - qlplayground
  - qlstephen # quick look extension-less text files
  - qlswift
  - qlvideo
  - reveal
  - sketch
  - skype #
  - sourcetree
  - spotify
  - steam
  - virtualbox # | https://www.virtualbox.org/
  - vlc
  - wordpresscom
  - zeplin

There are several more common cask apps listed in the playbook.yml - simply uncomment them to include them in your install.


### Packages/Utilities

Things installed with Homebrew:

  - bash # Bash 4
  - coreutils # Install GNU core utilities (those that come with OS X are outdated)
  - ctags
  - curl
  - docker # | https://docs.docker.com/installation/mac/
  - findutils  # Install GNU `find`, `locate`, `updatedb`, and `xargs`, g-prefixed
  - git
  - mas
  - openssl
  - rbenv # ruby. Just installs binaries - assumes you bring in the dotfiles.
  - readline
  - vim
  - wget
  - zsh

There are several more utils listed in the playbook.yml - simply uncomment them to include them in your install.


### System Settings

It also installs a few useful system preferences/settings/tweaks with a toned-down verson of Matt Mueller's [OSX-for Hackers script](https://gist.github.com/MatthewMueller/e22d9840f9ea2fee4716).

It does some reasonably gnarly stuff e.g.

  - hide spotlight icon
  - disable app Gate Keeper
  - change stand-by delay from 1hr to 12hrs.
  - Set trackpad tracking rate.
  - Set mouse tracking rate.
  - and lots more...

so you need read it very carefully first. (see scripts/system_settings.sh)

TODO: moar sick settings with https://github.com/ryanmaclean/OSX-Post-Install-Script


### User Preferences

It then syncs your user prefs with dotfiles+rcm

It grabs the [thoughttbot/dotfiles](https://github.com/thoughtbot/dotfiles) repo, saves it in `~/src/thoughtbot/dotfiles` and symlinks it to ~/dotfiles.

It then grabs [glennr/dotfiles](https://github.com/glennr/dotfiles) repo, saves it in `~/src/glennr/dotfiles` and symlinks it to ~/dotfiles-local

You probably want to change the `dotfile_repo_username` variable to match your github username :-)

It then runs rcup to initialize your dotfiles.



### MacStore Apps

These apps only available via the App Store. (sigh)
As long as you have already signed into the appstore app prior these will autoinstall

  - 443987910 # 1Password
  - 491200486 # iClarified
  - 405399194 # kindle
  - 604825918 # Valentina Studio
  - 603117688 # CCMenu
  - 407963104 # Pixelmator
  - 561995913 # Color maker
  - 937984704 # Amphetamine
  - 1039633667 # Irvue
  - 1037126344 # Apple Configurator 2
  - 803453959 # Slack
  - 497799835 # Xcode



### Application Settings (WIP)

Keep your application settings in sync.

TODO: Add Mackup task


### Other

- install fonts like a boss : http://lapwinglabs.com/blog/hacker-guide-to-setting-up-your-mac

## Development

You shouldn't wipe your entire workstation and start from scratch just to test changes to the playbook.

Instead, you can follow theses instructions for [how to build a Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which you can continually run and re-run this playbook to test changes and make sure things work correctly.

### Approach

We've tested it using an OSX 10.12 Vagrant/Virtualbox VM for developing & testing the Ansible scripts.

Simply spin up the Sierra box in a VM, and have vagrant kick off the laptop setup.

### Whats included?

Nada. Well not much. The whole point is to test the process of getting our OSX dev machines from zero to hero.

The Vagrant box we use is a [clean-ish install of OSX](https://github.com/timsutton/osx-vm-templates). However the setup notes above uses Packer, which installs Xcode CLI tools. This can't be automated in an actual test run, and needs user intervention to install.


### Test Setup

1. Get [Homebrew Cask](http://caskroom.io/)

        brew install caskroom/cask/brew-cask

1. Install VirtualBox;

        brew cask install --appdir="/Applications" virtualbox

### Notes

* VirtualBox doesn't have Guest additions for Mac OS X, so you can't have shared folders. Instead you can use normal network shared folders.

* If you are rolling your own box with [the OSX VM template](https://github.com/timsutton/osx-vm-templates), this is the Packer config;

      ```
      packer build \
        -var iso_checksum=aaaabbbbbbcccccccdddddddddd \
        -var iso_url=../out/OSX_InstallESD_10.10.4_14E46.dmg \
        -var update_system=0 \
        -var autologin=true \
        template.json
      ```

## Author

[Glenn Roberts](http://glenn-roberts.com), 2015.


## Credits

This project is based off the work of the following folks;

* Eduardo de Oliveira Hernandes' [ansible-macbook](https://github.com/eduardodeoh/ansible-macbook])
* Jeff Geerlings' [Mac Dev Ansible Playbook](https://github.com/geerlingguy/mac-dev-playbook)
* [Thoughtbot/laptop](https://github.com/thoughtbot/laptop) (boostrapping, dev tools)
* [OSX for Hackers](https://gist.github.com/MatthewMueller/e22d9840f9ea2fee4716) (awesome osx tweaks)
* [Mackup](https://github.com/lra/mackup)  (backup/restore App settings)

*See also*:

  - [Battleschool](http://spencer.gibb.us/blog/2014/02/03/introducing-battleschool) is a more general solution than what I've built here. (It may be a better option if you don't want to fork this repo and hack it for your own workstation...).
  - [osxc](https://github.com/osxc) is another more general solution, set up so you can fork the [xc-custom](https://github.com/osxc/xc-custom) repo and get your own local environment bootstrapped quickly.
  - [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks) was the original inspiration for this repository, but this project has since been completely rewritten.
