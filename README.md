# AWS EC2 SSH Autocompletion

A couple scripts to auto-populate your .ssh/config file with a list of hosts from your AWS_EC2 Name tags.

What these scripts will do, so please becareful.
  - Overwrite your home/.ssh/config file
  - Add autocompletion on MAC OSX in terminal:  ssh someserver
  - Magic


Note: I have a standard user I use on my instances whether it be my ldap user or the default ec2-user, this is set statically for all instances.  You will have to modify the updateSSH.sh file if you use another user. (ie: ubuntu, root, whatever)

### Installation

You need will need to have brew installed:
```sh
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

##### Install dependencies:
```sh
$ brew install bash-completion jq awscli
$ sudo easy_install pip
$ pip install boto
```

##### Clone the repo:
```sh
$ git clone https://github.com/chad-upton/AWS.EC2.SSH.Autocomplete.git
$ cd AWS.EC2.SSH.Autocomplete
```

##### Add your ec2 keypairs to your keychain
* Browse to your ec2_keypair.pem file that you created your instance with and right click -> open with keychain app

##### Add your EC2 Access Key & Secret Keys
Note: One thing you can do and most have done, is add these to your ~/.bashrc with lines 12-15 so not to have them stored in multiple places.  If you do that you will just need to source ~/.bashrc instead.
```sh
$ vim updateSSH.sh
Line6: Uncomment and edit: export AWS_ACCESS_KEY_ID='AK123'
Line7: Uncomment and edit: export AWS_SECRET_ACCESS_KEY='abc123'
$ :wq to save
```
##### Add Autocomplete to your home/.bashrc
```sh
vim ~/.bashrc
# Autocompletion
  if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
  fi
```

##### Copy ec2inventory to /usr/local/bin (optional but make note of paths in updateSSH.sh)
```sh
$ cp ec2inventory /usr/local/bin
```

##### Copy ec2.ini to /usr/local/bin (optional but again make note of paths)
```sh
$ cp ec2.ini /usr/local/bin
```

##### Run ec2inventory to make sure it's working
```sh
$ source updateSSH.sh (or ~/.bashrc)
$ ec2inventory
```

##### You should get a giant output of metadata about your ec2 environment.  If no errors then run:
```sh
$ updateSSH.sh
```
If you do get errors, double check your paths are correct and you've added the access/secret keys.

##### Check your ~/.ssh/config file for new entries
```sh
$ cat ~/.ssh/config
```

##### Should now see a list of hosts setup like this:
```sh
Host some_name_tag
    HostName some_private_ip
    User some_user
```

##### Test Autocompletion
```sh
$ ssh some_name_
```

### Notes

* The EC2inventory python script is borrowed from the ansible repository, if you want the latest you are welcome to download it from here: https://raw.githubusercontent.com/ansible/ansible/devel/plugins/inventory/ec2.py
* If you want public IP address instead of private IPs you can change it in the ec2.ini file

```sh
Line 38 to: ip_address
```

### Development

Want to contribute? Great!



