===
PWM
===


Raspbery pi
-----------

Kervi supports software generated pwm on the Raspberry pi's gpio pins.

.. code:: python
    
    from kervi.application import Application
    APP = Application()

    from kervi.hal import GPIO
    GPIO["GPIO10"].define_as_pwm()
    GPIO["GPIO10"].start_pwm(30)

PCA9685(I2C)
------------

PCA9685 is a 16 channel pwm generator that may be used to control LEDSs and servos.

.. code:: python

    PCA9685DeviceDriver(
        address=0x39, 
        bus=0
    )

:address: I2C address. Default is 0x39
:bus: I2C bus. If 0 or None default bus for platform is selected.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.pwm.PCA9685 import PCA9685DeviceDriver
    pwm = PCA9685DeviceDriver()
    
    #start 10% duty cycle and 40 Hz frequency  
    pwm[1].start_pwm(10, 40)

    #starts pwn and sets duty cycle to 100%
    pwm[1].value = True
    
    #stops pwm
    pwm[2].value = False

    #start pwm and set duty cycle to 50%
    pwm[3].value = .5

    pwn[3].link_to_dashboard()

    APP.run()
