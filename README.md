# Laptop

Laptop is a playbook to set up an OS X laptop (for web development).

It installs and configures most of the software Siyelo uses on our Macs for web and software development. 

It can be run multiple times on the same machine safely. It installs, upgrades, or skips packages based on what is already installed on the machine.

## Requirements

We've tested it on;

* OS X Yosemite (~10.10.4)

## Installation

### Fast Install

If you'd like to start with my default list of tools and apps (see Included Apps/Config below), then simply install with;

    curl --remote-name https://raw.githubusercontent.com/siyelo/laptop/master/mac
    sh mac 

You can always customize the install after-the-fact (see below), and re-run the playbook. It will skip over any installed apps.

### Custom Install

If you want to add/remove to the list of apps/utils installed, its pretty straightforward.

As above, download and bootstrap the script. But stop it before it starts ansible, and edit the playbook as desired, before re-running ansible.

1. Grab the bootstrap script 

        curl --remote-name https://raw.githubusercontent.com/siyelo/laptop/master/mac

2. Kick it off. Let it install the basics, and clone the repo

        sh mac

3. Stop the script (Ctrl+C) when ansible asks for the a 'sudo' password. 

        ```
        Changing to laptop repo dir ...

        Running ansible playbook ...
        SUDO password:  ^c

        ```

4. Change into the cloned repo dir

        cd laptop

5. Edit playbook.yml and add/remove the apps/utils you want. 

        vi playbook.yml

6. Kick off ansible manually

        ansible-playbook playbook.yml -i hosts --ask-sudo-pass -vvvv 

You can do this as many times as you like and re-run the `ansible-playbook` command. Ansible is smart enough to skip installed apps, so subsequent runs are super fast.


## Included Applications / Configuration

### Applications

Apps installed with Homebrew Cask:

TODO: update this list

  - Adium
  - BetterTouchTool
  - Google Chrome
  - Dropbox
  - Firefox
  - Handbrake
  - Homebrew
  - Karabiner
  - LICEcap
  - MacVim
  - Menu Meters
  - nvALT
  - Sequel Pro (MySQL client)
  - Skype
  - Skitch
  - Seil
  - Sublime Text
  - TextMate
  - TimeMachineEditor
  - Tower (Git client)
  - Transmit (S/FTP client)
  - Vagrant (+ Vagrant Manager)
  - VirtualBox
  - VLC

There are many more common cask apps listed in the playbook.yml - simply uncomment them to include them in your install. 

### MacStore Apps

These apps only available via the App Store. (sigh)

TODO: Port bork : https://github.com/mattly/bork/blob/master/types/macstore.sh and do this automagically!

  - 1Password
  - Monosnap
  - Pages
  - Keynote
  - Numbers
  - etc

### Packages/Utilities 
 
Things installed with Homebrew:

TODO: update this list

  - ansible
  - autoconf
  - gettext
  - libevent
  - packer
  - python
  - sqlite
  - mysql
  - php56 (+ php56-xdebug)
  - ssh-copy-id
  - cowsay
  - ios-sim
  - readline
  - subversion
  - kdiff3
  - openssl
  - pv
  - drush
  - wget
  - brew-cask

There are many more common utils listed in the playbook.yml - simply uncomment them to include them in your install. 


### Application Settings

Keep your application settings in sync.

TODO: Add Mackup task


### User Preferences

Keep your user prefs in sync

TODO: dotfiles - use rcm ?


### System Settings

Finally, there are a few other useful system preferences/settings/tweaks. 

  e.g. 

  6. Set trackpad tracking rate.
  7. Set mouse tracking rate.

TODO: Adapt OSX-for Hackers 
   - https://gist.github.com/MatthewMueller/e22d9840f9ea2fee4716
   - https://gist.github.com/brandonb927/3195465 


### Other 

- install fonts like a boss : http://lapwinglabs.com/blog/hacker-guide-to-setting-up-your-mac

- TODO: Install [Sublime Package Manager](http://sublime.wbond.net/installation).
- ZSH tab/auto completion
- Powerline in tmux
- zsh-autosuggestions plugin
- zsh-history-substring-search plugin
- zsh-notify plugin



## Development

You shouldn't wipe your entire workstation and start from scratch just to test changes to the playbook. 

Instead, you can follow theses instructions for [how to build a Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which you can continually run and re-run this playbook to test changes and make sure things work correctly.

### Approach

We've tested it using an OSX 10.10 Vagrant/Virtualbox VM for developing & testing the Ansible scripts.

Simply spin up the Yosemite box in a VM, and have vagrant kick off the laptop setup.

### Whats included?

Nada. Well not much. The whole point is to test the process of getting our OSX dev machines from zero to hero. 

The Vagrant box we use is a [clean-ish install of OSX](https://github.com/timsutton/osx-vm-templates). However the setup notes above uses Packer, which installs Xcode CLI tools. This can't be automated in an actual test run, and needs user intervention to install.


### Test Setup

1. Get [Homebrew Cask](http://caskroom.io/)
    
        brew install caskroom/cask/brew-cask

1. Install [Vagrant](https://www.vagrantup.com/downloads) 

        brew cask install --appdir="/Applications" vagrant

1. Install VirtualBox;

        brew cask install --appdir="/Applications" virtualbox

1. cd into this project directory;

1. Run 

        vagrant init http://files.dryga.com/boxes/osx-yosemite-10.10.3.0.box;

1. The Vagrantfile should be ready as soon as Vagrant downloads the box;

1. Start VM 

        vagrant up

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

