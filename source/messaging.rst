=========
Messaging
=========

It is possible to send messages from your kervi app when certain conditions occurs.
The main concept is that a message can be send to one or more message channels like e-mail and user log.
The user log is accessible in the UI and error and warning messages are shown as pop up notifications.

Value messages
==============

You can set warning and error ranges on sensors and other kervi values.

.. code:: python

    from kervi.application import Application
    app = Application()

    from kervi.sensors import Sensor
    from kervi.devices.sensors.dummy_sensor import DummySensorDeviceDriver

    sensor = Sensor("sensor","CPU", DummySensorDeviceDriver())
    
    sensor.link_to_dashboard(link_to_header=True)
    sensor.link_to_dashboard(type="radial_gauge")
    
    sensor.add_error_range((0, 10), "low error message", channels=["user_log", "email"])
    sensor.add_warning_range((10, 20), "low warning message")
    sensor.add_warning_range((80, 90), "high warning message",channels=["user_log", "email"])
    sensor.add_error_range((90, 100), "high error message", channels=["user_log", "email"])
    sensor.add_normal_range((20, 80), "normal ", channels=["user_log", "email"])
    
    app.run()


In the example above a series of error and warning ranges are defined. 
It is the channels parameter that controls where the range messages are send to.
Default value for the channels parameter is user_log if the parameter is omitted.

Sending custom messages
=======================

You use the Messaging class to send your own messages.


.. code:: python

    from kervi.messaging import Messaging

    Messaging.send_message("My message", level=1)

* level 1 is error.
* level 2 is warning.
* level 3 is normal info.


Configuration
=============

The message channels are activated via via :ref:`Kervi configuration <configuration>`.
The user log channel is always enabled. 

e-mail
------

Below is the required configuration for the email message plugin.
The plugin will be loaded if enabled is True.

.. code:: python

    from kervi.application import Application
    app = Application({
        "plugins":{
            "messaging":{
                "kervi.plugin.messaging.email": {
                    "enabled": False,
                    "smtp": {
                        "sender_name": "Kervi",
                        "sender_address": "kervi@example.com",
                        "server": "localhost",
                        "port": "25",
                        "user": "mail_user_name",
                        "password": "mail_user_password",
                        "tls": False
                    }
                }
            }
        }
    })

    app.run()
