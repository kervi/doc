======
Motors
======

This section shows how to initialize and link motor drivers in the kervi device library.

See :ref:`Motors <motors>` for further details on programming motors.


LN298
-----

LN298 is a 2 channal dc motor driver or one channel stepper driver.

.. code:: python

    from kervi.devices.motors.LN298 import PCA9685DeviceDriver
    LN298DeviceDriver(ena, in1, in2, enb, in3, in4, board_id="LN298", board_name="LN298")

:ena, in1, in2: gpio connections to respective pins on LN298 device for motor 1
:enb, in3, in4: gpio connections to respective pins on LN298 device for motor 2
:board_id: id used to reference this bord when using indirect linking.
:board_name: Name of board.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.hal import GPIO
    from kervi.devices.motor.LN298 import LN298DeviceDriver
    motor_driver = LN298DeviceDriver(
        GPIO["GPIO17"], GPIO["GPIO27"], GPIO["GPIO22"],
        GPIO["GPIO5"], GPIO["GPIO6"], GPIO["GPIO13"],
    )
    
    motor_driver.dc[0].speed = 20
    motor_driver.dc[1].speed = -20
    
    motor_driver.dc[0].speed.link_to_dashboard()
    motor_driver.dc[1].speed.link_to_dashboard()

    APP.run()
