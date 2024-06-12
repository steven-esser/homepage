**********************
Uncomplicated Firewall
**********************

:title: Uncomplicated Firewall
:date: 2020-08-17
:category: Posts
:tags: unix, devops

Uncomplicated Firewall (``ufw`` for short) is a user-friendly front end for creating
iptables rules. Its syntax makes it easy to open ports for different applications, while
still allowing for more complex filtering to take place. This guide will focus on
simplicity and should function as a good starting point for configuring ``ufw`` on internet
facing machines.

Installation
############
To install ``ufw``, run:
::

    $ sudo apt install ufw

``ufw`` should be available in most debian-based linux distribution's package repositories.
If you are running an rpm-based distro like fedora or Red Hat, you will need to check out
a different guide for their version of a "user-friendly" firewall: ``firewalld``. 

Now that the ``ufw`` package is installed, we can now proceed to configure the firewall and
add some simple rules.

Configuration
#############
Before creating any rules we will want to "reset" ``ufw`` to a known base state. Run the
following commands to set ``ufw`` to allow all outgoing connections and deny all incoming
connections:
::

    sudo ufw default deny incoming
    sudo ufw default allow outgoing

If you are setting up ``ufw`` on a laptop or desktop machine, this rule set may be enough.
However, we will need to add some additional rules if we want our machine to respond to
outside requests. 

In order to access your server remotely via ssh, we must configure ``ufw`` to allow ssh
connections from outside sources. Run the following command to allow ssh connections in
``ufw``:
::

    sudo ufw allow ssh

``ufw`` is smart enough to know which port ``ssh`` corresponds to. In fact, as long as the
service can be found in ``/etc/services``, ``ufw`` will be able to open the correct ports
just from the service name alone. 

Alternatively, ``ufw`` can accept plain port numbers as well. This is useful for custom
services and applications that are running on some arbitrary port. If we take the above
example, we can also all ssh connections via this command:
::

    sudo ufw allow 22

Enable and Start the Firewall
#############################
Once rules have been added for all the services you wish to access on your machine,
``ufw`` will need to be enabled as a service. To do this run the following command:
::

    sudo ufw enable

You may receive a warning about ssh connections being disrupted. However, this will not
be a problem if you set up the simple ssh rule mentioned in the above example. If this
ssh rule was not added prior to enabling ``ufw``, you may not be able to access the machine
remotely.

Finally, to verify ``ufw`` is running correctly, use to following command to display status
information:
::

    sudo ufw status

Conclusion
##########
``ufw`` should now be enabled and running correctly with a set of firewall rules. Even
these simple set of rules should provide a higher level of security than an otherwise
unconfigured system. ``ufw`` is also easier to set up and maintain than plain old
``iptables`` rules and its complex and unintuitive syntax. 

While this guide is just a simple introduction to the utility, ``ufw`` also has additional
functions that may be interesting for more complex use cases. Consult the ``man`` pages for
more information.
