# git2prov in a vagrant box

Information about git2prov: https://github.com/mmlab/Git2PROV

### Preparations and Installation

(OSX specific) The following commands can be used to prepare your local machine:

__Preparations__

```bash

# GET CASK
brew install caskroom/cask/brew-cask

# GET vagrant and virtualbox
brew cask install vagrant virtualbox

# Optionally install vagrant hostsupdater
vagrant plugin install vagrant-hostsupdater

# install ansible
brew install ansible

# add a ssh key to ~/.ssh
ssh-keygen -t rsa -b 4096 -C 'git2prov.dev'

# make sure to not add a passphrase by just hitting enter and saving the key 
# here: ~/.ssh/vagrant_rsa
# you will be needing it later on

# Add the following to your ssh-config located at ~/.ssh/config which creates a ssh configuration entry.
# for better readability the follwing is just appended to ~/.ssh/config file
#
# Host git2prov.dev
#   Hostname 192.168.34.11
#   Identityfile ~/.ssh/vagrant_rsa
#   StrictHostKeyChecking no
#   User root
#   LogLevel ERROR

echo 'Host git2prov.dev\n\tHostname 192.168.34.11\n\tIdentityfile ~/.ssh/vagrant_rsa\n\tStrictHostKeyChecking no\n\tUser root\n\tLogLevel ERROR \n\n' >> ~/.ssh/config
```

__Starting vagrant__

```bash

# change to the working directory where you cloned or copied this repository
cd /path/to/this-repository

# add a Ubuntu base box to be exact Trusty x64
# that might take a while, because you are downloading the image
vagrant box add ubuntu/trusty64

# when the boxes have finished downloading `vagrant up` will start a virtualmachine with ubuntu/trusty64
vagrant up
```


__Provision and orchestrate__

```bash

# change to the ansible folder 
cd ./data/ansible

# we are assuming the single host vagrant environment 
#
# first of all we need to fetch the ansible roles we depend on
ansible-galaxy install -r requirements.yml -f

# then start the playbook
ansible-playbook install.yml

```

## License

Copyright (c) Andreas Ruck under the MIT license.
