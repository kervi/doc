=======
Sensors
=======

Sensors are an important part of automation. 
Below you see the app from previous section extended with code that includes a  
sensor for cpu load on the device where the script is executed.

.. code-block:: python
    :linenos:
    
    if __name__ == '__main__':

        from kervi.application import Application
        
        app = Application()

        #create sensor
        from kervi.sensors import Sensor
        from kervi.devices.sensors.system import CPULoadSensorDeviceDriver

        #create a senors that uses CPU load device driver
        cpu_load_sensor = Sensor("CPULoadSensor","CPU", CPULoadSensorDeviceDriver())
        
        #link to dashboard
        cpu_load_sensor.link_to_dashboard("*", "header_right")
        cpu_load_sensor.link_to_dashboard(type = "value", show_sparkline=True, link_to_header=True)
        cpu_load_sensor.link_to_dashboard(type="chart")

        app.run()


Use Ctrl+C in the terminal window if your application is still running and run the script again.
Your app should now look like this.

.. image:: ../images/hello_sensors.png

The UI is responsive and responds when you scale the browser window or view it on a mobile phone.

.. image:: ../images/hello_sensors_responsive.png
    :width: 35 %


You can see the sensor device drivers that are available out of the box in the :ref:`kervi device library <kdl_sensors>`

Read more about sensors :ref:`here <device_sensors>`.

Proceed to the next chapter and lean how to react to sensor changes.