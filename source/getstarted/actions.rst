=======
Actions
=======

Kervi actions are used when you want to react to sensors, gpio ports and other kinds of input.
You create an action by decorating a function with the @action decorator.

The code below defines two actions one that is linked to a sensor 
and one that is linked to the web ui.

.. code-block:: python

    .. code-block:: python

    if __name__ == '__main__':

        from kervi.application import Application
        
        app = Application()

        #create sensor
        from kervi.sensors.sensor import Sensor
        from kervi.devices.platforms.common.sensors.cpu_use import CPULoadSensorDeviceDriver

        #link to dashboard
        cpu_load_sensor.link_to_dashboard("*", "header_right")
        cpu_load_sensor.link_to_dashboard(type = "value", show_sparkline=True, link_to_header=True)
        cpu_load_sensor.link_to_dashboard(type="chart")

        from kervi.actions import action
        
        #create an action that listen to a sensor
        @action
        def my_sensor_action():
            print("My sensor action called")

        #link action to sensor 
        #trigger action when sensor value is greater than 25%
        my_sensor.link_to(cpu_load_sensor, trigger_value: lambda x: x > 25)


        #create an action that is linked to the ui.
        @action
        def my_ui_action():
            print("my ui action called")

        my_ui_action.link_to_dashboard()

        app.run()


When you run the code above you should see output in the terminal window when the actions are executed.

When you run your kervi script on an Raspberry Pi it is possible to link an action to gpio.
Connect a switch to GPIO 12 on the Raspberry Pi and run the script below.
Each time the switch is pressed the my_gpio_action is called and the action text is printed in the terminal window. 

.. code-block:: python

    .. code-block:: python

    if __name__ == '__main__':

        from kervi.application import Application
        
        app = Application()

        from kervi.hal import GPIO
        GPIO["GPIO12"].define_as_input()
    
        from kervi.actions import action

        @action
        def my_gpio_action()
            print("my gpio action called")

        my_gpio_action.link_to(GPIO["GPIO12"])


Read more about all the possibilities with actions :ref:`here <actions>`.

 