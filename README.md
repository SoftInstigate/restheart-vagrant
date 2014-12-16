# Vagrant box for RESTHeart #

This is a complete Vagrant box for [RESTHart](http://restheart.org). It uses the [Ansible provisioner](https://docs.vagrantup.com/v2/provisioning/ansible.html).

**Tested with:**

 * Virtualbox `4.3.20`
 * Vagrant `1.7.0`
 * Mongodb `2.6.6`
 * RESTHeart `0.9.3`

The Vagrant box is a plain Ubuntu Trusty64 (14.04 LTS). The provisioning process adds:

 * Java 8
 * MongoDB
 * RESTHeart Java API Server

## Setup ##

**Note**: You must have at least [Python](https://www.python.org/downloads/) 2.6+ installed in your host. Ansible doesn't support Python 3 yet. Python 2.x is installed by default on most Linux versions and Mac OSX. For Windows you can go [here](https://www.python.org/downloads/release/python-279/).

 1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
 1. Install [Vagrant](https://www.vagrantup.com/downloads.html) 1.6+
 1. Install [Ansible](http://docs.ansible.com/intro_installation.html).

Ansible can be universally installed via `pip`, the Python package manager. If `pip` isn’t already available in your distribution of Python, you can get pip by:

    $ sudo easy_install pip

Then install Ansible with:

    $ sudo pip install ansible


## Start the Vagrant box ##

 1. Clone this Git repository, it already contains the latest RESTHeart JAR file under the `restheart-0.9.3` folder.
 1. `cd` into the cloned repository folder
 1. run `vagrant up --provision`. It will take several minutes, depending on your Internet connection, as it needs to download and install the JDK 8 and all MongoDB packages.

 ## The RESTHeart Service ##

RESTHeart is installed in background as a system service named `restheartd`, which starts automatically at boot. It reads at startup the `restheart.yml` and `security.yml` configuration files present in `restheart-0.9.3/etc` folder. You can `tail -f restheart.log` in the `/vagrant` shared folder to verify that everything is up and running. The output should be similar to:

    23:46:26.086 [main] INFO  c.s.restheart.Bootstrapper - starting RESTHeart ********************************************
    23:46:26.086 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart version 0.9.3
    23:46:26.219 [main] INFO  c.s.restheart.Bootstrapper - initializing mongodb connection pool to 127.0.0.1:27017 
    23:46:26.222 [main] INFO  c.s.restheart.Bootstrapper - mongodb connection pool initialized
    23:46:27.298 [main] INFO  c.s.restheart.Bootstrapper - https listener bound at 0.0.0.0:4443
    23:46:27.299 [main] INFO  c.s.restheart.Bootstrapper - http listener bound at 0.0.0.0:8080
    23:46:27.305 [main] INFO  c.s.restheart.Bootstrapper - local cache enabled
    23:46:27.411 [main] INFO  c.s.restheart.Bootstrapper - url / bound to mongodb resource *
    23:46:27.594 [main] INFO  c.s.restheart.Bootstrapper - embedded static resources browser extracted in /tmp/restheart-4821268372483417156
    23:46:27.609 [main] INFO  c.s.restheart.Bootstrapper - url /browser bound to static resources browser. access manager: false
    23:46:27.612 [main] INFO  c.s.restheart.Bootstrapper - url /_logic/ping bound to application logic handler com.softinstigate.restheart.handlers.applicationlogic.PingHandler. access manager: false
    23:46:27.621 [main] INFO  c.s.restheart.Bootstrapper - url /_logic/roles/mine bound to application logic handler com.softinstigate.restheart.handlers.applicationlogic.GetRoleHandler. access manager: false
    23:46:27.925 [main] INFO  c.s.restheart.Bootstrapper - logging to /vagrant/restheart.log with level INFO
    23:46:27.925 [main] INFO  c.s.restheart.Bootstrapper - stopping logging to console 
    23:46:27.926 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart started **********************************************

To control the service, you can `vagrant ssh` into the guest box and type

    sudo service restheartd [stop|start|restart]

## Use RESTHeart ##

Connect to [`http://localhost:8080/browser`](http://localhost:8080/browser) to access the HAL browser. 

For authentication (see the `security.yml` configuration file):

 * user: `admin`
 * password: `changeit`

Then have a look at the complete [documentation](http://restheart.org/docs/overview.html).

If you have the MongoDB client installed locally you could also access the database directly from its default port `27017`, simply using the `mongo` command from your host.

