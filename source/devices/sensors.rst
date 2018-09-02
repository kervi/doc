=======
Sensors
=======

This section shows how to initialize and link sensors in the kervi device library.

See :ref:`Sensors <device_sensors>` for further details on sensor programming.

-----------
Temperature
-----------


.. _BMP085:

BMP085(I2C)
-----------

.. code:: python

    BMP085DeviceDriver(
        sensor_type=BMP085_TEMPERATURE_SENSOR, 
        mode=BMP085_STANDARD, 
        address=BMP085_I2CADDR, 
        bus=0
    )

:sensor_type: BMP085_TEMPERATURE_SENSOR, BMP085_PRESSURE_SENSOR or BMP085_ALTITUDE_SENSOR
:mode: BMP085_ULTRALOWPOWER, BMP085_STANDARD, BMP085_HIGHRES, BMP085_ULTRAHIGHRES
:address: I2C address. Default is 0x77
:bus: I2C bus. If 0 or None default bus for platform is selected.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.sensors.BMP085 import BMP085DeviceDriver, BMP085_TEMPERATURE_SENSOR
    temp = Sensor("temp_1", "Temperature outdoor", BMP085DeviceDriver(BMP085_TEMPERATURE_SENSOR))
    temp.link_to_dashboard()

    APP.run()


DS18B20(One wire)

.. code:: python

    DS18XXXDeviceDriver(Address)

:address: One wire address

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.sensors.DS18XXX import DS18XXXDeviceDriver
    temp = Sensor("temp_1", "Temperature outdoor", DS18XXXDeviceDriver("0xFC2B454C040010"))
    temp.link_to_dashboard()

    APP.run()

--------
Pressure
--------

BMP085
------
    see :ref:`BMP085`.




-----------
Orientation
-----------

LSM9DS0(I2C)
------------

This driver combines all sensors in the LSM9DS0 into one sensor with three dimentions, heading(compass), roll and pitch

.. code:: python

    LSM9DS0OrientationDeviceDriver(
        is_flipped=False, 
        accl_address=I2C_ACCL_ADDRESS, 
        gyro_address=I2C_GYRO_ADDRESS, 
        bus=None
    )

:is_flipped: True if sensor is phycically flipped.
:accl_address: I2C address for accelerator sensor.
:gyro_address: I2C address for gyro sensor.
:bus: I2C bus

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.sensors.LSM9DS0 import LSM9DS0OrientationDeviceDriver
    sensor = Sensor("orientation", "Orientation", LSM9DS0OrientationDeviceDriver())
    
    #access sub sensors
    #compass
    sensor[0].link_to_dashboard()
    #roll
    sensor[1].link_to_dashboard()
    #pitch
    sensor[2].link_to_dashboard()

    APP.run()