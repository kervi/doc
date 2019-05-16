GPIO
====

Kervi standardize access to GPIO (general-purpose input/output).
It is possible to link GPIO pins to dashboards, controllers and actions 

GPIO main board
--------------

Access to the mainboard (Raspberry PI) GPIO is done via the kervi.hal module. When your Kervi application starts it scans
for installed Kervi platform drivers and loads the GPIO driver.

.. code-block:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["GPIO12"].define_as_input()

        #Link to dashboard it will show as a read only switch.
        #If the the GPIO 12 pin is set high, the switch will change to on. 
        GPIO["GPIO12"].link_to_dashboard(")

        GPIO["GPIO13"].define_as_output()
        #Link to dashboard it will show as a switch
        #Press the button on screen to turn the GPIO pin 12 high
        GPIO["GPIO13"].link_to_dashboard("main", "gpio")

        APP.run()

PWM
---

On Raspberry pi there is software emulated PWM.
Below is GPIO03 defined as pwm. The duty cycle is linked to the dashboard 

.. code-block:: python

    if __name__ == '__main__':
    from kervi.application import Application
    APP = Application()

    #Get access to mainboard GPIO
    from kervi.hal import GPIO

    GPIO["GPIO3"].define_as_pwm()
    
    #Link to dashboard 
    GPIO["GPIO3"].pwm["duty_cycle"].link_to_dashboard()

    @action
    def app_main():
        GPIO["GPIO3"].pwm["duty_cycle"].value=50
    
    APP.run()


Other GPIO devices
------------------

GPIO on an Rasberry pi digital in/out. 
If you want to use analog in out or extent the number of digital pins you will have to use a GPIO chip like the PCF8591.

.. code:: python

    from kervi.application import Application
    APP = Application()

    from kervi_devices.gpio import PCF8591

    PCF8591_gpio = PCF8591.PCF8591Driver()
    PCF8591_gpio["AIN0"].link_to_dashboard()

    APP.run()

Scheduling
----------

It is possible to schedule when to change an gpio pin.

.. code:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        from kervi.hal import GPIO

        GPIO["GPIO12"].define_as_output()

        GPIO["GPIO12"].every().monday.at("10:30").set(True)
        GPIO["GPIO12"].every().monday.at("11:30").set(False)

        GPIO["GPIO12"].every(2).monday.at("10:30").set(True)
        GPIO["GPIO12"].every(2).monday.at("11:30").set(False)

        GPIO["GPIO12"].every().day.at("10:30").set(True)
        GPIO["GPIO12"].every().day.at("11:30").set(False)

        GPIO["GPIO12"].every().minute.at(":00").set(True)
        GPIO["GPIO12"].every().minute.at(":30").set(False)

        APP.run()

Linking to dashboards
---------------------

A gpio pin can be linked to a dashboard in the web ui.
The ui component presented depends on the gpio type.
The web ui user can change the state of the gpio pin it is defined as output other wise the ui element is read only. 

.. code-block:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["GPIO12"].define_as_input()
        GPIO["GPIO12"].link_to_dashboard(title="button 1", type="switch")

        GPIO["GPIO11"].define_as_output()
        GPIO["GPIO11"].link_to_dashboard(title="Light 1", type="button")


Linking to controllers
----------------------


Linking to actions
------------------

.. code:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["GPIO12"].define_as_input()

    
        from kervi.actions import action

        @action
        def my_action(p):
            print("my action", p)


        #my action is called when gpio12 goes high 
        my_action.link_to(GPIO["GPIO12"])

        APP.run()