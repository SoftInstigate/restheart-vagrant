# Vagrant box for RESTHeart #

This is a complete Vagrant box for [RESTHart](http://restheart.org) REST API Server. It uses the [Ansible provisioner](https://docs.vagrantup.com/v2/provisioning/ansible.html).

> RESTHeart is the REST API server for [MongoDB](https://www.mongodb.org). It is written in **Java 8** and built on top of the [Undertow](http://undertow.io) non-blocking Web server. It exposes all its functions via HTTP and allows, for example, to fully manage a MongoDB database with just `curl` or from a JavaScript client running in a browser. 

**Tested with:**

 * Virtualbox `4.3.20`
 * Vagrant `1.7.0`
 * MongoDB `2.6.6`
 * RESTHeart `0.9.5`

The Vagrant box is a plain Ubuntu Trusty64 (14.04 LTS). The provisioning process adds:

 * Java 8
 * MongoDB
 * RESTHeart Java API Server

## Setup ##

> You must have at least [Python](https://www.python.org/downloads/) 2.6+ installed in your host. Ansible doesn't support Python 3 yet. Python 2.x is installed by default on most Linux versions and Mac OSX. For Windows you can go [here](https://www.python.org/downloads/release/python-279/).

 1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
 1. Install [Vagrant](https://www.vagrantup.com/downloads.html) 1.6+
 1. Install [Ansible](http://docs.ansible.com/intro_installation.html).

Ansible can be universally installed via `pip`, the Python package manager. If `pip` isn’t already available in your distribution of Python, you can get pip by:

    $ sudo easy_install pip

Then install Ansible with:

    $ sudo pip install ansible


## Start the Vagrant box ##

 1. Clone this Git repository
 1. `cd` into the cloned repository folder
 1. run `vagrant up --provision`. It will take several minutes, depending on your Internet connection, as it needs to download and install the JDK 8, all MongoDB packages and the **latest binary release of RESTHeart**.

 > In case the mongod service doesn't start automatically, you could either run `vagrant provision` again or log-in the VM (`vagrant ssh`) and issue `sudo service mongod start`

 ## The RESTHeart Service ##

RESTHeart is installed in background as a system service named `restheartd`, which starts automatically at boot (it uses the `restheartd.conf` configuration file).

You can `tail -f tmp/restheart.log` in the `/vagrant` shared folder to verify that everything is up and running. The output should be similar to:

    15:30:46.250 [Thread-2] INFO  c.s.restheart.Bootstrapper - stopping RESTHeart
    15:30:46.256 [Thread-2] INFO  c.s.restheart.Bootstrapper - waiting for pending request to complete (up to 1 minute)
    15:32:21.267 [main] INFO  c.s.restheart.Bootstrapper - starting RESTHeart ********************************************
    15:32:21.279 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart version 0.9.5
    15:32:21.413 [main] INFO  c.s.restheart.Bootstrapper - initializing mongodb connection pool to 127.0.0.1:27017 
    15:32:21.416 [main] INFO  c.s.restheart.Bootstrapper - mongodb connection pool initialized
    15:32:22.042 [main] WARN  c.s.restheart.Bootstrapper - ***** no identity manager specified. authentication disabled.
    15:32:22.043 [main] WARN  c.s.restheart.Bootstrapper - ***** no access manager specified. users can do anything.
    15:32:22.391 [main] INFO  c.s.restheart.Bootstrapper - https listener bound at 0.0.0.0:4443
    15:32:22.391 [main] INFO  c.s.restheart.Bootstrapper - http listener bound at 0.0.0.0:8080
    15:32:22.397 [main] INFO  c.s.restheart.Bootstrapper - local cache enabled
    15:32:22.441 [main] INFO  c.s.restheart.Bootstrapper - url / bound to mongodb resource *
    15:32:22.770 [main] INFO  c.s.restheart.Bootstrapper - embedded static resources browser extracted in /tmp/restheart-7371469076573689136
    15:32:22.789 [main] INFO  c.s.restheart.Bootstrapper - url /browser bound to static resources browser. access manager: false
    15:32:23.129 [main] INFO  c.s.restheart.Bootstrapper - logging to /tmp/restheart.log with level INFO
    15:32:23.130 [main] INFO  c.s.restheart.Bootstrapper - logging to console with level INFO
    15:32:23.130 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart started **********************************************

To control the service, you can `vagrant ssh` into the guest box and type

    sudo service restheartd [stop|start|restart]

## Use RESTHeart ##

If everything went fine, a running RESTHeart server is listening at port 8080 and you could verify that by connecting to the embedded [HAL browser](http://localhost:8080/browser). 

Then have a look at the complete [documentation](http://restheart.org/docs/overview.html) for the next steps.
