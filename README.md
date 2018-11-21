# packer_mysql64

### Purpose of the repository 
- The repository should have the minimal code for creating Ubuntu box with `Packer` and should install `mysql` on it.

#### List of files in the repository

- `http/preseed.cfg` - script that automatically installs Ubuntu.
- `scripts/provision.sh` - script that installs `mysql`.
- `template.json` - template with code for `packer` to create the image we want.
- `test/integration/default/check_pkg.rb` - script needed by `kitchen` to check if `mysql` is installed. 
- `test/integration/default/gen.sh` - script needed by `kitchen` to check if `mysql` is installed. 
- `.gitignore` - which files and directories to ignore.
- `.kitchen.yml` - testing framework used by `ruby`.
- `Gemfile` - used for `ruby` dependencies.

### How to use this repository. 
- Install `virtualbox` by following this [instructions](https://www.virtualbox.org/wiki/Downloads).
- Install `vagrant` by following this [instructions](https://www.vagrantup.com/docs/installation/).
- Install `packer` by following this [instructions](https://www.packer.io/intro/getting-started/install.html).
- Clone the repository to your local computer: `git clone git@github.com:nikcbg/packer_mysql64.git`.
- Go to the cloned repo on your computer: `cd packer_mysql64`.
- Execute `packer validate template.json` to validates `template.json` file, after executing the command it should return `Template validated successfully` message. 
- Execute `packer build template.json`  to start building the virtual machine you need to run your tests on. 
- After that you should see this message `mysql64-vbox: 'virtualbox' provider box: mysql64-vbox.box` which means that the VM box was created successfully.
- Execute `vagrant box list` to see the list of `vagrant` boxes.
- Execute `vagrant box add --name mysql64 mysql64-vbox.box` to add the newly created `packer` box. 
- Execute `vagrant init mysql64` to create Vagrantfile if one doesn't already exist.  
- Execute `vagrant up` command to power up the VM.
- Execute `vagrant ssh` to log in to the VM.
- Execute `sudo service mysql status` to see if `mysql` server is installed and runing, the output will display the following:
```
mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2018-11-20 11:36:07 UTC; 25min ago
  Process: 778 ExecStartPost=/usr/share/mysql/mysql-systemd-start post (code=exited, status=0/SUCCESS)
  Process: 753 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
 Main PID: 777 (mysqld)
   CGroup: /system.slice/mysql.service
           └─777 /usr/sbin/mysqld
```
- Execute `exit` to exit the VM and start testing with `kitchen`.

### Setting up `ruby` environment on Ubuntu 18.04 instructions:
- Before testing with `kitchen` you need to install and prepare `ruby` environment.
- Execute `sudo apt update` to update the packages on your Ubuntu computer. 
- Execute `sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev` to install `ruby` dependencies.
- Execute `wget -q https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer -O- | bash` to download and install `rbenv` environment
- Execute `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile` to change your `~/.bashrc` file to use `ruby` command line utility 
- Execute `echo 'eval "$(rbenv init -)"' >> ~/.bashrc` so `rbenv` loads automatically.
- Execute `source ~/.bash_profile` to reload `bash` profile.
- Execute `type rbenv` command to verify that `rbenv` is set up properly, the output will display the following:
```
rbenv is a function
rbenv ()
{
    local command;
    command="${1:-}";
    if [ "$#" -gt 0 ]; then
        shift;
    fi;
    case "$command" in
        rehash | shell)
            eval "$(rbenv "sh-$command" "$@")"
        ;;
        *)
            command rbenv "$command" "$@"
        ;;
    esac
}
```

- Execute `rbenv install 2.5.3` to install `ruby 2.5.3` version.
- Execute `rbenv local 2.5.3` to set the default version of `ruby` to your local directory.
- Execute `rbenv -v` to make sure `ruby` is installed and you have the correct version.
- Execute `gem install bundler` to install `gem` which is package manager for `ruby`, the output will display the following:
```
Successfully installed bundler-1.17.1
1 gem installed
```

### Commands needed to test with `kitchen`.
- Execute `bundle exec kitchen list` to list `kitchen` instances.
- Execute `bundle exec kitchen converge` to create `kitchen` environment.
- Execute `bundle exec kitchen verify` command to execute `kitchen` test.
- Execute `bundle exec kitchen destroy` to destroy `kitchen` environment.
- Execute `bundle exec kitchen test` to automatically build, test and destroy `kitchen` environment.

### TO DO:
- Check if `mysql` server is installed and running. 
