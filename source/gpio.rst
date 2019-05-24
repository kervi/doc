.. _gpio:
====
GPIO
====

Kervi standardize access to GPIO (general-purpose input/output).
It is possible to link GPIO pins to dashboards, controllers and actions 

GPIO main board
---------------

Access to the mainboard (Raspberry PI) GPIO is done via the kervi.hal module. When your Kervi application starts it scans
for installed Kervi platform drivers and loads the GPIO driver.

.. code-block:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["gpio21"].define_as_input()

        #Link to dashboard it will show as a read only switch.
        #If the the GPIO 21 pin is set high, the switch will change to on. 
        GPIO["gpio21"].link_to_dashboard()

        GPIO["gpio20"].define_as_output()
        #Link to dashboard it will show as a switch
        #Press the button on screen to turn the GPIO pin 20 high
        GPIO["gpio10"].link_to_dashboard()

        APP.run()

Pull up/down
-----------

When a gpio pin is not connected electrically to a low og hi signal it may float at take arbitrary values.
To avoid that the gpio pins a pulled down when not connected.
Some times it is desireable to pull up the pin when not connected.

.. code-block:: python

    GPIO["gpio21"].define_as_input(pull_up=True)

Bounce
------

When a physical switch or button is pressed the metal connectors in the switch makes microscopic bounces until
the switch is locked in the new position. 
The result of this bouncing may be several small signals on the gpio pin going on or off within a few milliseconds.
To prevent that a pin should hold its signal for certain amount of time before triggering a value change. 
When defining an gpio input its is possible to set the time for bouncing and the default value is 200 milliseconds. 

.. code-block:: python
    
    GPIO["gpio21"].define_as_input(bounce_time=300)

PWM
---

On Raspberry pi there is software emulated PWM.
Below is GPIO03 defined as pwm. The duty cycle is linked to the dashboard.
The duty cycle is a value between 0 and 100.

.. code-block:: python

    if __name__ == '__main__':
    from kervi.application import Application
    APP = Application()

    #Get access to mainboard GPIO
    from kervi.hal import GPIO

    #define pin as pwm with a frequency of 60 Hz and an initial duty_cycle=50%
    GPIO["gpio3"].define_as_pwm(60, 50)
    GPIO["gpio3"].pwm.active.value = True
    
    #Link to dashboard 
    GPIO["gpio3"].pwm.duty_cycle.link_to_dashboard()

    @action
    def app_main():
        #change the duty cycle. The value should be
        GPIO["gpio3"].pwm.duty_cycle.value=80
    
    APP.run()

Other GPIO devices
------------------

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

        GPIO["gpio21"].define_as_output()

        GPIO["gpio21"].every().monday.at("10:30").set(True)
        GPIO["gpio21"].every().monday.at("11:30").set(False)

        GPIO["gpio21"].every(2).monday.at("10:30").set(True)
        GPIO["gpio21"].every(2).monday.at("11:30").set(False)

        GPIO["gpio21"].every().day.at("10:30").set(True)
        GPIO["gpio21"].every().day.at("11:30").set(False)

        GPIO["gpio21"].every().minute.at(":00").set(True)
        GPIO["gpio21"].every().minute.at(":30").set(False)

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

        GPIO["gpio21"].define_as_input()
        GPIO["gpio21"].link_to_dashboard(title="button 1", type="switch")

        GPIO["GPIO11"].define_as_output()
        GPIO["GPIO11"].link_to_dashboard(title="Light 1", type="button")


Linking to controllers
----------------------





Linking to actions
------------------

GPIO pins can trigger actions.

.. code:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["gpio21"].define_as_input()
   
        from kervi.actions import action

        @action
        def my_action():
            print("my action")


        #my action is called when gpio21 goes high 
        my_action.link_to(GPIO["gpio21"])

        APP.run()


Call an action when the value change and pass that value

.. code:: python

    from kervi.application import Application
    APP = Application()

    from kervi.actions import action

    @action
    def my_action(v):
        print("my action", v)

    from kervi_devices.gpio import PCF8591

    PCF8591_gpio = PCF8591.PCF8591Driver()
    
    #Call action when ever the value on AIN0 changes and pass the value to the action. 
    my_action.link_to(PCF8591_gpio["AIN0"], pass_value=True)
    APP.run()


Action interrupts
-----------------

Below is an example where an action is executed when gpio the pin is high and exists when the gpio pin goes low.

.. code:: python

    from kervi.application import Application
    APP = Application()

    #Get access to main board GPIO
    from kervi.hal import GPIO
    GPIO["gpio21"].define_as_input()

    from kervi.actions import action
    import time

    @action
    def my_action():
        print("my action start")

        #The action decorator injects "exit_action"
        while not exit_action:
            print("Do action things")
            time.sleep(.5)

        print("my action stop")

    my_action.link_to(GPIO["gpio21"], interrupt_enabled=True)