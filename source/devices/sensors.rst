=======
Sensors
=======

This section shows how to initialize and link sensors in the kervi device library.

See :ref:`Sensors <device_sensors>` for further details on sensor programming.

-----------
Temperature
-----------

CPU temp
--------

.. code:: python

    from kervi.devices.platforms.common.cpu_temp import CPUTempSensorDeviceDriver
    CPUTempSensorDeviceDriver()

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.platforms.common.cpu_temp import CPUTempSensorDeviceDriver
    temp = Sensor("cpu_temp", "CPU temp", CPUTempSensorDeviceDriver())
    temp.link_to_dashboard()

    APP.run()


.. _BMP085:

BMP085(I2C)
-----------

.. code:: python

    from kervi.devices.sensors.BMP085 import BMP085DeviceDriver
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
-----------------

.. code:: python

    from kervi.devices.sensors.DS18XXX import DS18XXXDeviceDriver
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

This driver combines all sensors in the LSM9DS0 into one sensor with three dimensions, heading(compass), roll and pitch

.. code:: python

    from kervi.devices.sensors.LSM9DS0 import LSM9DS0OrientationDeviceDriver
    LSM9DS0OrientationDeviceDriver(
        is_flipped=False, 
        accl_address=I2C_ACCL_ADDRESS, 
        gyro_address=I2C_GYRO_ADDRESS, 
        bus=None
    )

:is_flipped: True if sensor is physically flipped.
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



-------
Battery
-------


CW2015 (I2C, Rpi power pack hat)
---------------------------

CW2015 is a single channel Lipo battery manager where it is possible to read the voltage and capacity of the battery.

.. code:: python

    from kervi.application import Application
    APP = Application()

    from kervi.sensors.sensor import Sensor
    from kervi.devices.sensors.CW2015 import CW2015VoltageDeviceDriver, CW2015CapacityDeviceDriver
    
    sensor = Sensor("CW2015_voltage", "CW2015 voltage", CW2015VoltageDeviceDriver())
    sensor.link_to_dashboard()

    sensor1 = Sensor("CW2015_capacity", "CW2015 capacity", CW2015CapacityDeviceDriver())
    sensor1.link_to_dashboard()

    APP.run()

-----------
Light (lux)
-----------

TSL2561(I2C)
------------

.. code:: python

    from kervi.devices.sensors.TSL2561 import TSL2561DeviceDriver
    TSL2561DeviceDriver(
        address=0x39, 
        bus=0
    )

:address: I2C address. Default is 0x39
:bus: I2C bus. If 0 or None default bus for platform is selected.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.sensors.TSL2561 import TSL2561DeviceDriver
    lux = Sensor("lux_1", "Light", TSL2561DeviceDriver()
    lux.link_to_dashboard()

    APP.run()

------
Other
------

Memory
------

Sensor that measures the memory use on the device.

.. code:: python

    from kervi.devices.platforms.common.memory_use import MemUseSensorDeviceDriver
    MemUseSensorDeviceDriver()

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.platforms.common.cpu_temp import MemUseSensorDeviceDriver
    temp = Sensor("mem_use", "Memory use", MemUseSensorDeviceDriver())
    temp.link_to_dashboard()

    APP.run()

CPU load
--------

Sensor that measures the cpu load.

.. code:: python

    from kervi.devices.platforms.common.cpu_use import CPULoadSensorDeviceDriver
    CPULoadSensorDeviceDriver()

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.platforms.common.cpu_load import CPULoadSensorDeviceDriver
    cpu_load = Sensor("cpu_load", "CPU load", CPULoadSensorDeviceDriver())
    cpu_load.link_to_dashboard()

    APP.run()

Disk use
--------

Sensor that measures disk use.

.. code:: python

    from kervi.devices.platforms.common.disk_use import DiskUseSensorDeviceDriver
    DiskUseSensorDeviceDriver()

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.platforms.common.disk_use import DiskUseSensorDeviceDriver
    disk_use = Sensor("disk_use", "Disk use", DiskUseSensorDeviceDriver())
    disk_use.link_to_dashboard()

    APP.run()
