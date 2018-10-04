=======
Actions
=======

Kervi actions are use when you want to react to sensors, gpio ports and other kinds of input.
You create an action by decorating a function with the @action decorator.

The fan controller below code is extended with an action called Active.
That action is linked to the ui where it is possible to toggle the fan controller on/off.

.. code-block:: python
  :linenos:

    if __name__ == '__main__':

        from kervi.application import Application
                
        app = Application()

        #create sensors
        from kervi.sensors.sensor import Sensor
        from kervi.devices.platforms.common.sensors.cpu_use import CPULoadSensorDeviceDriver
        from kervi.devices.platforms.common.sensors.cpu_temp import CPUTempSensorDeviceDriver

        #create a senors that uses CPU load device driver
        cpu_load_sensor = Sensor("CPULoadSensor","CPU", CPULoadSensorDeviceDriver())
        
        #link to dashboard
        cpu_load_sensor.link_to_dashboard("*", "header_right")
        cpu_load_sensor.link_to_dashboard(type = "value", show_sparkline=True, link_to_header=True)
        cpu_load_sensor.link_to_dashboard(type="chart")

        #create a senors that uses CPU temp device driver
        cpu_temp_sensor = Sensor("CPUTempSensor","CPU temp", CPUTempSensorDeviceDriver())
        
        #link to dashboard
        cpu_temp_sensor.link_to_dashboard("*", "header_right")
        cpu_temp_sensor.link_to_dashboard(type = "value", show_sparkline=True, link_to_header=True)
        cpu_temp_sensor.link_to_dashboard(type="chart")

        from kervi.controllers.controller import Controller
        from kervi.values import NumberValue
        from kervi.actions import action
        
        class FanController(Controller):
            def __init__(self):
                Controller.__init__(self, "fan_controller", "Fan")

                #define input
                self.temp = self.inputs.add("temp", "Temperature", NumberValue)
                self.temp.min = 0
                self.temp.max = 150
                
                #define output
                self.fan_speed = self.outputs.add("fan_speed", "Fanspeed", NumberValue)

                self._active = False

            #action that toggles the monitoring of the temperature input on/off
            @action
            def active(self):
                self._active = not self._active
                self._calc_fan_speed()

            def input_changed(self, changed_input):
                _calc_fan_speed()

            def _calc_fan_speed():
                if self._active:
                    temp = self.temp.value
                    if temp <= 20:
                        self.fan_speed.value = 0
                    else:
                        speed = (temp / 80) * 100
                        if speed > 100:
                            speed = 100
                        self.fan_speed.value = speed
                else:
                    self.fan_speed.value = 0

        
        #Instantiate controller and link it to ui and devices
        fan_controller = FanController()

        #show the controller input and output in the ui.
        fan_controller.temp.link_to_dashboard()
        fan_controller.fan_speed.link_to_dashboard()

        #link the fan controllers temp input to cpu temperature sensor
        fan_controller.temp.link_to(cpu_temp_sensor)
        
        #link to the motor controller device
        from kervi.devices.motors.adafruit_i2c_motor_hat import AdafruitMotorHAT
        motor_driver = AdafruitMotorHAT()
        motor_driver.dc_motors[0].speed.link_to(fan_controller.speed)

        #link to ui.
        fan_controller.active.link_to_dashboard()

        

        app.run()

Read more about all the possibilities with actions :ref:`here <actions>`.