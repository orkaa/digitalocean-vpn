Digitalocean-vpn
================

Digitalocean-vpn is a tool written in bash that makes it easy to create a digitalocean droplet running Openvpn. Client config can be easily exported and used in a tool like Tunnelblick.

Digitalocean-vpn makes it possible to run the instance only when needed. Once you have it running, you can `sleep` and then `wake` it. While sleeping, droplet doesn't exist - only its snapshot does. Snapshot storage is currently free (written on November, 2015)!

Prerequisites
=============

* A machine running OS X or Linux (currently only tested on OS X)
* Digitalocean account and a generated API key for this application
* SSH keypair for connecting to the instance (in current version, ~/.ssh/id_rsa.pub is automatically used)

Digitalocean-vpn relies heavily on digitalocean's API which uses REST to communicate. Since json is hard to parse just with bash, a single dependency is needed.

**OS X**
```
brew install jq
```

**Debian/Ubuntu**
```
apt-get install jq
```

Usage
=====

First you'll need a config file holding the API key for digitalocean.

```
./digitalocean-vpn
```

On first run, this will create a config in `~/.digitalocean-vpn`. Open it and add your digitalocean API key.

Create the droplet
------------------

```
./digitalocean-vpn create
```

Once the droplet is created, you will be able to extract the config to be used in Tunnelblick ...

Get the Openvpn config
----------------------

```
$ ./digitalocean-vpn config > ~/Desktop/digitalocean-vpn.ovpn
```

If you have Tunnelblick installed, you can just double-click the config and it will automatically configure itself.

Sleep
-----

Put the droplet to sleep when you don't need it. Droplet will be deleted but snapshotted.

```
$ ./digitalocean-vpn sleep
```

Wake
----

Droplet can be started from the snapshot without the need to reconfigure tunnelblick with a new config.

```
$ ./digitalocean-vpn wake
```

Status
------

Check if the droplet is running and the state of its snapshot.

```
$ ./digitalocean-vpn status
```

Destroy
-------

Get rid of both droplet and snapshot if either exists.

```
$ ./digitalocean-vpn destroy
```

Other notes
===========

If digitalocean-vpn fails when creating the droplet for the first time, your best bet is to just run `./digitalocean-vpn destroy` and then retry the create.

If it fails repeatedly, you have an option to run in debug mode. You'll be able to see all commands currently being executed.

**To run in debug mode**
```
$ ./digitalocean-vpn create --debug
```

Currently, everything about the droplet is hardcoded. The smallest available instance is used in region nyc3. I'm going to change this soon, allowing it to be configured from ~/.digitalocean-vpn



