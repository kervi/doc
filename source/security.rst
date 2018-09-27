========
Security
========

.. warning::

    The security features of the Kervi framework has not been reviewed by any external experts and
    use is at own risk. 
    
    Exposing your Kervi application directly on the internet by routing traffic from internet router to the Kervi web server 
    is strongly advised against at the moment.

    At this moment of development passwords and user names are sent in clear text between browser and Kervi web server
    unless you enable TLS encryption (see below).

It is possible to secure your Kervi application with user authentication and encrypted communication between browser 
and the kervi web server.

User access in the ui is controlled via :ref:`Kervi configuration <configuration>` in the plugins/authentication section

Simple user management
======================

Kervi comes with a simple authentication plugin where users are defined in config.
You enable this plugin by setting kervi.plugin.authentication.plain to true.

.. code:: python

    "plugins":{
        "authentication": {
            "kervi.plugin.authentication.plain": True
        }

This plugin looks for uses in the plain_users section.

.. code:: python

    "plain_users": {
            "anonymous":{
                "enabled": True,
                "groups":[]
            },
            "admin":{
                "enabled": True,
                "password":"",
                "groups":["admin"],
                "name": "Administrator",
                "addresses": {
                    "email": "admin@example.com",
                    "phone": ""
                }
            }
        }

As seen above each user is defined whit the user name as key in the configuration section and
the password and groups as parameters for each user.

You can use the groups in your python code to define who has access to dashboards, panels and sensors.

In the snippet below the cpu sensor is only visible to users who belong to the admin group

.. code:: python

    CPU_SENSOR = Sensor("CPULoadSensor","CPU", CPULoadSensorDeviceDriver(), user_groups=["admin"])

In the same way dashboards and panels may be restricted to certain groups as shown below.

.. code:: python

    Dashboard(
        "system",
        "System",
        [
            DashboardPanel("power", title="Power", user_groups=["admins"])
        ],
        user_groups=["admins", "users"]
    )

In the snippet above users that is member of the groups *admins* or *users* have access to the system dashboard 
but it is only members in the *admins* groups that can see the power panel.

Anonymous access
----------------

Anonymous access is controlled via the anonymous user in the plain_users configuration section. 

 .. code:: python
     
    "plain_users": {
        "anonymous":{
            "enabled": True
        }
    }
    
    

The web user will be prompted with a login screen when accessing the UI if anonymous access is disabled.
If anonymous access is enabled the web ui will try to login as anonymous user when the users go to the UI web page. 
All components that does not have the property user_groups set will be visible to the anonymous user.
In addition, components that belongs to any user groups specified for the anonymous user will be visible too.  

.. note::

    Authentication only apply to the web users. 
    Internal communication in the kervi applications python code is not affected by user authentication.
    All components can query and command each other plus receive events. 


Encryption
==========

To encrypt communication between browser and Kervi web server a certificate is needed. 
Encryption is enabled via :ref:`Kervi configuration <configuration>` in the encryption section.  

.. code:: python

    "encryption":{
        "enabled": true,
        "use_ssl": false,
        "cert_file": "path to cert file",
        "key_file":  "path to key file"
    }

