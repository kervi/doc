.. _controllers:
==============
Controllers
==============

A controller reacts to one or more inputs and computes one or more outputs.
The input could come from the user via the web based UI, sensors or other application logic.

Basic controllers manages motors, light, servos and so on but you can also build more advanced controllers
for *line following* and *self navigation*.

Below is an example of a steering controller, that takes speed, and left/right balance between motors as inputs
and calculates the speed of the left and right motors.

.. code-block:: python
   :linenos:

    from kervi.controllers.controller import Controller
    from kervi.values import *

    class MotorSteering(Controller):
        """
        Control the speed and direction of two motors.
        """
        def __init__(self, controller_id="steering", name="Steering"):
            Controller.__init__(self, controller_id, name)
            self.type = "steering"
            
            #create the inputs
            # speed value is from -100 to 100.
            self.speed = self.inputs.add("speed", "Speed", NumberValue)

            #direction - -100 is max left, 0 is strait ahead, 100 is max right 
            self.direction = self.inputs.add("direction", "Direction", NumberValue)

            #create outputs
            self.left_speed = self.outputs.add("left_speed", "Left speed", NumberValue)
            self.right_speed = self.outputs.add("right_speed", "Right speed", NumberValue)

        def update(self):
            new_direction = self.direction.value

            if left_right_balance > 0:
                left_speed = speed * (1 - new_direction / 100)
                right_speed = speed
            elif left_right_balance < 0:
                left_speed = speed
                right_speed = speed * (1 + new_direction / 100)
            elif left_right_balance == 0:
                left_speed = speed
                right_speed = speed

            self.left_speed.value = left_speed
            self.right_speed.value = right_speed

        def input_changed(self, changed_input):
            if changed_input == self.speed or changed_input == self.direction:
                self.update()

    #instantiate controller
    steering = MotorSteering()
        
    #Link to ui
    steering.speed.link_to_dashboard("app", "steering")
    steering.direction.link_to_dashboard("app", "steering")
    steering.all_off.link_to_dashboard("app", "steering")

    #link to hardware
    motor_board = AdafruitMotorHAT()
    motor_board.dc_motors[2].speed.link_to(steering.left_speed)
    motor_board.dc_motors[3].speed.link_to(steering.right_speed)

In the example above the inputs and outputs are defined via::

    self.speed = self.inputs.add("speed", "Speed", NumberValue)
    
    self.left_speed = self.outputs.add("left_speed", "Left speed", NumberValue)

self.inputs.add and self.outputs.add are factory functions that creates kervi values that are special value classes that 
may be linked to dashboards or to another dynamic values.

When an input value is changed by a user or other part of your application the input_changed event is fired and your controller may
calculate its outputs. 

A controller that works entirely on in- and outputs is agnostic to how it is linked to user interface and hardware.
In that way it is easy to change hardware and make changes to UI without re-coding the controller.

