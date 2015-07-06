# Vagrant boxes for RESTHeart #

This project enables a set of two distinct Vagrant boxes for [RESTHart](http://restheart.org) REST API Server and MongoDB. It uses the [Ansible provisioner](https://docs.vagrantup.com/v2/provisioning/ansible.html).

> RESTHeart is the REST API server for [MongoDB](https://www.mongodb.org). It is written in **Java 8** and built on top of the [Undertow](http://undertow.io) non-blocking Web server. It exposes all its functions via HTTP and allows, for example, to fully manage a MongoDB database with just `curl` or from a JavaScript client running in a browser. 

**Tested with:**

 * Virtualbox `4.3.x`
 * Vagrant `1.7.x`
 * MongoDB `3.0.x`
 * RESTHeart `0.10.x`

The two Vagrant boxes are both plain Ubuntu Trusty64 (14.04 LTS). The provisioning process adds two boxes, one named `db-vm` and one names `api-vm`.

1) db-vm (at IP `192.168.50.4`) with
 * MongoDB

2) api-vm (at IP `192.168.50.5`) with
 * Java 8
 * RESTHeart Java API Server

> We have decided to split the previous monolithic single VM in two distinct boxes because this makes the provisioning and maintenance easier, as you can upgrade MongoDB and RESTHeart separately. A side benefit is that you can run the `db` box as a standalone MongoDB VM now, but the first creation is slightly longer as two Vagrant boxes need to be created. However, now you can destroy either the `db` or `api` VM, which is nice if you really want to delete all data without re-provisioning the RESTHeat front-end.

## Setup ##

> You must have at least [Python](https://www.python.org/downloads/) 2.6+ installed in your host. Ansible doesn't support Python 3 yet. Python 2.x is installed by default on most Linux versions and Mac OSX. For Windows you can go [here](https://www.python.org/downloads/release/python-279/).

 1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
 1. Install [Vagrant](https://www.vagrantup.com/downloads.html) 1.6+
 1. Install [Ansible](http://docs.ansible.com/intro_installation.html).

Ansible can be universally installed via `pip`, the Python package manager. If `pip` isn't already available in your distribution of Python, you can get pip by:

    $ sudo easy_install pip

Then install Ansible with:

    $ sudo pip install ansible

## Start the Vagrant boxes ##

 1. Clone this Git repository
 1. `cd` into the cloned repository folder
 1. run `vagrant up --provision`. It will take several minutes, depending on your Internet connection, as it needs to download and install the JDK 8, all MongoDB packages and the **latest binary release of RESTHeart**.

 The provisioning process adds two entries in `/etc/hosts` in your Linux or Mac OSX host machine (if you run Vagrant on Windows it will be different, but we have not tested this with Windows yet).

 *Note*: In this phase you might be required to enter your host's password, so that the file can be updated.

> Usually we have found that the mongod service does not bind correctly after creation, so you can log-in the VM (`vagrant ssh db`) and issue `sudo service mongod restart`. This happens only once after the very first `vagrant up`.

Anyway, most errors appearing on the very first startup usually disappear after a halt and restart (`vagrant reload`). When everything is up and running then you don't need to re-provision anymore, unless a new RESTHeart version is available, but the process will selectively re-install only what is new.

To control the two boxes separately you need to issue the usual Vagrant commands but ending with either `db` or `api`. For example: `vagrant halt api` stops the RESTHeart front-end server only.

We have added an `halt.sh` script to gracefully shutdown both VMs, but you could just issue a `vagrant halt`. However always remember to shutdown the MongoDB instance (db VM) at last, so that RESTHeart can flush data against a still running MongoDB instance while shutting down.

## The RESTHeart Service ##

RESTHeart is installed in background as a Upstart system service named `restheartd`, which starts automatically at boot (it uses the `restheartd.conf` configuration file).

You can `tail -f tmp/restheart.log` in the `/vagrant` shared folder to verify that everything is up and running. The output should be similar to:

    15:30:46.250 [Thread-2] INFO  c.s.restheart.Bootstrapper - stopping RESTHeart
    15:30:46.256 [Thread-2] INFO  c.s.restheart.Bootstrapper - waiting for pending request to complete (up to 1 minute)
    15:32:21.267 [main] INFO  c.s.restheart.Bootstrapper - starting RESTHeart ********************************************
    15:32:21.279 [main] INFO  c.s.restheart.Bootstrapper - RESTHeart version 0.10.0
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

To control the service, you can `vagrant ssh api` into the `api` guest box and type

    sudo service restheartd [stop|start|restart]

## Use RESTHeart ##

If everything went fine, a running RESTHeart server is listening at port 8080 of the `api` VM while MongoDB will listen to port 27017 of the `db` VM. Verify that by connecting to the embedded [HAL browser](http://localhost:8080/browser). 

Please have a look at the complete [documentation](http://restheart.org/docs/overview.html) for the next steps or, in case of problems, check our [Support page](http://restheart.org/support.html).
