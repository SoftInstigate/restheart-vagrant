# Vagrant box for RESTHeart #

This is a complete Vagrant box for [RESTHart](http://restheart.org). It uses the [Ansible provisioner](https://docs.vagrantup.com/v2/provisioning/ansible.html).

Tested with Virtualbox `4.3.20` and Vagrant `1.7.0`.

The Vagrant box is a plain Ubuntu Trusty64 (14.04 LTS). The provisioning process adds:

 * Java JDK 8
 * MongoDB server (2.6 at present)
 * RESTHeart Java API Server

## Setup ##

**Note**: You must have [Python](https://www.python.org/downloads/) 2.6+ installed in your host. Ansible doesn't support Python 3 yet. Python 2.x is installed by default on most Linux versions and Mac OSX. For Windows you can go [here](https://www.python.org/downloads/release/python-279/).

 1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
 1. Install [Vagrant](https://www.vagrantup.com/downloads.html) 1.6+
 1. Install [Ansible](http://docs.ansible.com/intro_installation.html). Probably the most common way is via [Python's pip](http://docs.ansible.com/intro_installation.html#latest-releases-via-pip)

Ansible can be installed via `pip`, the Python package manager. If `pip` isn’t already available in your version of Python, you can get pip by:

    $ sudo easy_install pip

Then install Ansible with:

    $ sudo pip install ansible


## Start the Vagrant box ##

 1. Clone this Git repository, it already contains the latest RESTHeart JAR file under the `restheart-0.9.3` folder.
 1. `cd` into the cloned repository folder
 1. run `vagrant up --provision`. It will take a while as needs to download and install both JDK 8 and MongoDB.

 ## Run the RESTHeart Service ##

To start RESTHeart in background run the `start.sh` command. You can `tail -f restheart.log` to verify that everything is up and running. The output should be similar to:

    11:37:20.322 [main] INFO  c.s.restheart.Bootstrapper - starting RESTHeart ********************************************
    11:37:20.334 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart version 0.9.3
    11:37:20.472 [main] INFO  c.s.restheart.Bootstrapper - initializing mongodb connection pool to 127.0.0.1:27017 
    11:37:20.475 [main] INFO  c.s.restheart.Bootstrapper - mongodb connection pool initialized
    11:37:21.122 [main] WARN  c.s.restheart.Bootstrapper - ***** no identity manager specified. authentication disabled.
    11:37:21.123 [main] WARN  c.s.restheart.Bootstrapper - ***** no access manager specified. users can do anything.
    11:37:21.550 [main] INFO  c.s.restheart.Bootstrapper - https listener bound at 0.0.0.0:4443
    11:37:21.552 [main] INFO  c.s.restheart.Bootstrapper - http listener bound at 0.0.0.0:8080
    11:37:21.559 [main] INFO  c.s.restheart.Bootstrapper - local cache enabled
    11:37:21.612 [main] INFO  c.s.restheart.Bootstrapper - url / bound to mongodb resource *
    11:37:21.929 [main] INFO  c.s.restheart.Bootstrapper - embedded static resources browser extracted in /tmp/restheart-5638391690370374596
    11:37:21.955 [main] INFO  c.s.restheart.Bootstrapper - url /browser bound to static resources browser. access manager: false
    11:37:22.366 [main] INFO  c.s.restheart.Bootstrapper - logging to /tmp/restheart.log with level INFO
    11:37:22.367 [main] INFO  c.s.restheart.Bootstrapper - logging to console with level INFO
    11:37:22.368 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart started **********************************************

To stop the service, just kill the java process.

## Use RESTHeart ##

Connect to [`http://localhost:8080/browser`](http://localhost:8080/browser) to access the HAL browser.
Then have a look at the [documentation](http://restheart.org/docs/overview.html).
