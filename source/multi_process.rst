====================
Process distribution
====================

In python it is possible to have multiple threads in a process but only one is executed at any given time (due to the infamous gil lock).
This may impact the performance of your kervi application if you have a lot of sensors, cameras and controllers in your (single file) application.
Kervi makes it easy to overcome this limitation by spreading your code over more than one process and thereby makes the separated process code truly concurrent.
Another advantage is that the underlaying operative system may spread the processes over available cpu cores and that makes your application more responsive.  
This is done by creating python modules in sub folders relative to your main kervi python script.

The kervi framework uses its interprocess communications system(IPC) to connect the different processes in your application and transmit sensor values and call actions commands across process boundaries.

The kervi framework spawns two processes of its own, one for ipc and one for the web server that servers the dashboards. 

Belove is the folder structure of a kervi project with multiple python modules

.. code::

    app root
    |- controllers 
        |- __init__.py
        |- fan_controller.py
    |- sensors
        |- __init__.py
        |- system_sensor.py
    |- myapp.py

The kervi application is bootstrapped in myapp.py where the modules parameter tells kervi 
which modules to load. 

.. code:: python
    
    from kervi.application import Application
    import kervi.utility.nethelper as nethelper

    if __name__ == '__main__':
        
        APP = Application({
            "modules":["sensors", "controllers"],
        })

        @action
        def app_main():
            Actions["fan_controller.activate", True]

        @action
        def app_exit():
            Actions["fan_controller.activate", False]

        APP.run()

When the Application.run method is called the modules in the "modules parameter" are loaded into separate processes.
In each of the loaded modules the __init__.py file bootstraps and load the components that resides in the local folder.

When the application has loaded the app_main action is called by the framework and the fan controller is started. 
Likewise the app_exit action is called by the framework when the application terminates.

In this example the fan controller has its own action Activate that sets the state of the controller. 
The fan controller lives in a processes separate from your myapp.py code. 
Kervi detects this and transmits the action over the IPC system to the controller module code.

The sensors module could look like this:

sensors/__init__.py

.. code:: python

    from . import system_sensors

sensors/system_sensors.py

.. code:: python

    from kervi.sensors.sensor import Sensor
    from kervi.devices.platforms.common.sensors.cpu_use import CPULoadSensorDeviceDriver
    
    #create a sensor with id "cpu_load"
    CPU_SENSOR = Sensor("cpu_load","CPU", CPULoadSensorDeviceDriver())
    CPU_SENSOR.link_to_dashboard(type="value", link_to_header=True)
    CPU_SENSOR.link_to_dashboard(type="chart")

The controllers module contains the following code.

controllers/__init__.py

.. code:: python

    from . import fan_controller

controllers/fan_controller.py

..code:: python

    from kervi.controllers.controller import Controller
    from kervi.values import NumberValue
    from kervi.action import action

    class FanController(Controller):
        def __init__(self):
            Controller.__init__(self, "fan_controller", "Fan")

            self.temp = self.inputs.add("temp", "Temperature", NumberValue)
            self.temp.min = 0
            self.temp.max = 150
            
            self.fan_speed = self.outputs.add("fan_speed", "Fanspeed", NumberValue)

            self._active = False

        @action
        def activate(self, active)
            self._active = active
            self._calc_speed()
        
        def input_changed(self, changed_input):
            self._calc_speed()

        def _calc_speed(self)
            if self_active:
                temp = self.temp.value
                if temp <= 20:
                    self.fan_speed.value = 0
                else:
                    speed = (temp / 80) * 100
                    if speed > 100:
                        speed = 100
                    self.fan_speed.value = speed
            else:
                self.fan_speed.value = speed

    FAN_CONTROLLER = FanController()

    #link the fan controllers temp input to cpu temperature sensor
    #The temp sensor is loaded in another process and linked via its id
    FAN_CONTROLLER.temp.link_to("cpu_load")
    

It is the last line in the example above that makes the inter process link between the sensor process and the controller process.
The kervi frame work detects that the sensor with id "cpu_load" does not live in this process and starts to listen
for value events from the sensor that are transmitted via IPC.
