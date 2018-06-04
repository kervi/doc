.. _dynamic_values-label:

===============
Signal handling
===============

The KerviValue class is a core class in the kervi framework that handles monitoring and flowing of data inside the application. 

A KerviValue has the following properties:

* Holds one value of type int, float, string, boolean and dict.
* It exists in two modes input or output. A *kervi value* in input mode is changeable and a value in output mode is readable.
* An *input value* can link to *output value* in order to monitor changes in the output value. This link exists accross process and device boundaries.
* It is possible to link a kervi value to dashboard and panels. The visible representation depends on value type and input mode.  

Value linking
-------------

Value linking is the process of connect an output KerviValue to a value that is input.

*Direct linking*

The example below shows how to make a self balancing robot. This is done as direct linking where all values are
in the same process.

The orientation of the robot is obtained by a LSM9DS0 sensor.
The output of the sensor is linked to a pid controller that calculates
how much the wheels must be moved to keep the robot in balance.
The result of the pid controller is sent to the steering controller that calculates the speed of each motor
and sends the calculated motor speeds to the motor controller.

.. code:: python

    #imports an initialization of the application is omitted for clarity
    
    #create orientation sensor
    orientation_sensor = Sensor("orientation", "Orientation", LSM9DS0OrientationDeviceDriver(), reading_interval=.1)

    #create pid controller
    pid_controller = PIDController("balance_pid", "Balance pid")
    
    #link sensor roll value and pid controller
    pid_controller.value.link_to(orientation_sensor[2])
    
    #create the steering controller
    steering = MotorSteering()
    #link adaptive_speed controller input to steering controller
    steering.adaptive_speed.link_to(pid_controller.result)

    #create motor controller
    motor_board = AdafruitMotorHAT()
    #link dc motors to the steering controller outputs
    motor_board.dc_motors[2].speed.link_to(steering.left_speed)
    motor_board.dc_motors[3].speed.link_to(steering.right_speed)

*Indirect linking*

It is not possible to use direct linking if the two values that should
be connected are in different processes or maybe on different devices/computers. 
In this situation it is possible use utilize the builtin inter process communication system
in Kervi.

In the snippet below the link is done by passing the id of the sensor in the call to link_to.
The input value then starts to listen for value changes that are broardcast over IPC by the output.  

An indirect link must be established on the input side in the link.

Process one code

.. code:: python

    #create orientation sensor
    orientation_sensor = Sensor("orientation", "Orientation", LSM9DS0OrientationDeviceDriver(), reading_interval=.1)


process two code

.. code:: python

    pid_controller = PIDController("balance_pid", "Balance pid")
    pid_controller.value.link_to("orientation.roll")

Value link transformation
-------------------------

It is possible to transform the values that are passed in the link by apply a transformation
function or lambda expression. This is useful if the value ranges of the two
 values are not the same or if you need to change the sign of the
values that are exchanged in the link.

Below is an example where a servo motor is mounted opposite to the on screen pan
controller. By changing the sign the in the link the servo will rotate as expected on
the screen.

.. code:: python 

    motor_board.servo_motors[0].position.link_to(CAM1.pan, lambda x: -x)


Dashboard linking
-----------------

Kervi values may be linked to dashboards.
When linked they will show up as either inputs that the user can change or
a read only representation. 

Details about dashboard linking are found in the dashboard section.  
