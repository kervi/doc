====
GPIO
====

Access to the GPIO on the mainboard (Raspberry PI) is handled via the GPIO class.

.. code-block:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        #To work you must import it after you have create the application
        from kervi.hal import GPIO

        GPIO["gpio21"].define_as_input()

        #Link to dashboard it will show as a read only switch.
        #If the the GPIO 21 pin is set high, the switch will change to on. 
        GPIO["gpio121"].link_to_dashboard()

        GPIO["gpio20"].define_as_output()
        #Link to dashboard it will show as a switch
        #Press the button on screen to turn the GPIO pin 20 high
        GPIO["gpio20"].link_to_dashboard()

        APP.run()

You can also set the value in code

.. code-block:: python

    if __name__ == '__main__':
        from kervi.application import Application
        APP = Application()

        #Get access to main board GPIO
        from kervi.hal import GPIO

        GPIO["gpio21"].define_as_output()

        from kervi.action import action
        import time

        #This action is called when the system has loaded
        @action
        def app_main()
            GPIO["gpio21"].value = True
            time.sleep(2)
            GPIO["gpio21"].value = False
        
        APP.run()

Read more about all the possibilities with gpio :ref:`here <gpio>`.