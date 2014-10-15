:title: Vagrant + Chef-Solo
:author: Fernando Espíndola
:description: Hands-on demonstration of vagrant and chef-solo
:css: vagrant.css

----

Vagrant and Chef-Solo
=====================

.. image:: images/vagrant-logo.gif
    :height: 250px

.. image:: images/chef-logo.png
    :height: 250px

----

:data-x: r+1100
:data-y: 0

Get and Install Vagrant
=======================

https://www.vagrantup.com/downloads.html

.. code:: bash

    $ vagrant --version
    Vagrant 1.6.5

.. note::

    * Check version: vagrant --version

----

Install VirtualBox
==================

.. code:: bash

    $ sudo apt-get install virtualbox

    $ vboxmanage --version
    4.3.10_Ubuntur93012

.. image:: images/virtualbox-logo.png
    :height: 400px

----

Project Setup
=============

.. code:: bash

    $ vagrant init

This command will create a *Vagrant* file with the minimun required configuration.

.. note::

    The primary function of the Vagrantfile is to describe the type of machine required for a project, and how to configure and provision these machines.

    - Remove comments
    
    config.vm.box     = "precise32"

    config.vm.box_url = "http://files.vagrantup.com/precise32.box"

----

Up and Running
==============

.. code:: bash

    $ vagrant up

----

Other Commands
==============

.. code:: bash

    $ vagrant status
    $ vagrant ssh
    $ vagrant destroy

----

Provisioning
============

    * **Chef (Chef-Solo)**

    * Puppet
    
    * Shell

    * Etc.

https://docs.vagrantup.com/v2/provisioning/index.html

----

Chef-Solo
=========

Cookbook
--------

    * unit of configuration for distribution

    * scenarios (components that are required)

    * Chef maintains a collection of cookbooks

https://community.opscode.com/cookbooks-directory

----

:data-x: r+0
:data-y: r+1100

Chef-Solo
=========

Installation
------------

.. code:: bash

    # curl -L https://www.opscode.com/chef/install.sh | bash
    # chef-solo --version
    Chef: 11.16.4

----



Chef-Solo
=========

Initial Chef Configuration
--------------------------

.. code:: bash

    $ wget http://github.com/opscode/chef-repo/tarball/master
    $ tar -zxf master
    $ mv opscode-chef-repo* chef-repo
    $ rm master

.. note::

    $ cd chef-repo

    $ ls

    Resource: http://gettingstartedwithchef.com/

----

Knife Configuration
===================

.. code:: bash

    $ mkdir .chef
    $ echo "cookbook_path [ 'cookbooks' ]" > .chef/knife.rb

.. note::
    
    * Add to the file: .chef/knife.rb

        * http_proxy "http://proxy:port"
    
        * https_proxy "http://proxy:port"

----

Vagrant Plugins
===================

Install the following plugins

.. code:: bash

    $ vagrant plugin install vagrant-omnibus
    $ vagrant plugin install vagrant-proxyconf

Add to the *Vagrantfile*

.. code:: ruby

    # config.omnibus.chef_version = :latest
    config.omnibus.chef_version = "11.16.4"

    config.proxy.http  = "http://proxy:port/"
    config.proxy.https = "http://proxy:port/"

----

Getting Cookbooks
=================

Our first Chef cookbook
-----------------------

**apt**
-------

.. code:: bash

    $ knife cookbook site download apt
    $ tar zxf apt*
    $ rm apt*.tar.gz

----

Getting Cookbooks
=================

Our first Chef cookbook
-----------------------

**apt**
-------

.. code:: bash

    $ knife cookbook site download virtualenvwrapper
    $ tar zxf virtualenvwrapper*
    $ rm virtualenvwrapper*.tar.gz

----

Getting Cookbooks
=================

**redis**
---------

.. code:: bash

    $ knife cookbook site download redis
    $ tar zxf redis*
    $ rm redis*.tar.gz

----

Add Cookbooks to Vagrant
========================

.. code:: ruby

    config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "chef-repo/cookbooks"
      chef.data_bags_path = "chef-repo/data_bags"
      chef.add_recipe "apt"
      chef.add_recipe "virtualenvwrapper"
      chef.add_recipe "redis::install_from_package"
    end

----

:data-x: r-1100
:data-y: r+0

Get Dependencies
================

.. code:: bash

    $ knife cookbook site download python
    $ tar zxf python*

    $ knife cookbook site download runit
    $ tar zxf runit*
    $ knife cookbook site download install_from
    $ tar zxf install_from*
    $ knife cookbook site download metachef
    $ tar zxf metachef*

    $ knife cookbook site download build-essential
    $ tar zxf build-essential*
    $ knife cookbook site download yum
    $ tar zxf yum*
    $ knife cookbook site download yum-epel
    $ tar zxf yum-epel*

----

Providers
=========

https://github.com/mohitsethi/vagrant-hp

.. image:: images/hp-helion-logo.png
    :height: 130px

https://github.com/mitchellh/vagrant-aws

.. image:: images/aws-logo.png
    :height: 130px

----

Putting it all together
=======================

----

:data-x: 3800
:data-y: 3800
:data-scale: 10
:data-rotate-z: 0
:data-rotate-x: 180
:data-rotate-y: 0
:data-z: 0

Thank you!
===========
Fernando Espíndola
------------------

+------------------------------------+-----------------------------------------+
| .. image:: images/gmail-logo.jpg   |  fer.esp@gmail.com                      |
|         :height: 20px              |                                         |
+------------------------------------+-----------------------------------------+
| .. image:: images/twitter-logo.jpg | `@feresp <https://twitter.com/feresp>`_ |
|         :height: 35px              |                                         |
+------------------------------------+-----------------------------------------+

On Github 
---------
https://github.com/fernandoe/training-vagrant
https://github.com/fernandoe/paineldabolsa-server
