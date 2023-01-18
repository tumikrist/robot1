## til að stýra robot
til að stýra robotinum notar maður vinstri stýri pinnan til að fara framm og tilbaka. 
og hægri stýri pinnin er notaður til að beyggja til hægri og vinstri.
og takkarinr vinstra megin aftan á eru notaðir til að loka og opna klóna.
og svo takkarnir aftan á hægra megin láta arminn fara upp og niður.

## link á video af robotinum
https://youtube.com/shorts/HwgUXiDe4GU

## kóði
``` cpp
  /*----------------------------------------------------------------------------*/
/*                                                                            */
/*    Module:       main.cpp                                                  */
/*    Author:       VEX                                                       */
/*    Created:      Tue Oct 01 2019                                           */
/*    Description:  Clawbot Control                                           */
/*                  This program uses Controller events to drive the arm and  */
/*                  claw of the V5 Clawbot.                                   */
/*                                                                            */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// Controller1          controller                    
// LeftMotor            motor         1               
// RightMotor           motor         10              
// ArmMotor             motor         8               
// ClawMotor            motor         3               
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;

// Declare Contoller event callbacks.
void whenControllerL1Pressed() {
  // Lift the ArmMotor.
  ArmMotor.spin(forward);
  // Wait until L1 is released.
  waitUntil(!Controller1.ButtonL1.pressing());
  // Stop the motor
  ArmMotor.stop();
}
void whenControllerL2Pressed() {
  // Lower the ArmMotor.
  ArmMotor.spin(reverse);
  // Wait until L2 is released.
  waitUntil(!Controller1.ButtonL2.pressing());
  // Stop the motor
  ArmMotor.stop();

}
void whenControllerR1Pressed() {
  // Open the ClawMotor.
  ClawMotor.spin(reverse);
  // Wait until R1 is released.
  waitUntil(!Controller1.ButtonR1.pressing());
  // Stop the motor
  ClawMotor.stop();

}
void whenControllerR2Pressed() {
  // Close the ClawMotor.
  ClawMotor.spin(forward);
  // Wait until R2 is released.
  waitUntil(!Controller1.ButtonR2.pressing());
  // Stop the motor
  ClawMotor.stop();
}


int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();

  //task myTask = task(stopTakki);

  // Initialize the Contoller Events
  Controller1.ButtonL1.pressed(whenControllerL1Pressed);
  Controller1.ButtonL2.pressed(whenControllerL2Pressed);
  Controller1.ButtonR1.pressed(whenControllerR1Pressed);
  Controller1.ButtonR2.pressed(whenControllerR2Pressed);


  // Set the brake mode and velocity of the ArmMotor and ClawMotor
  ClawMotor.setStopping(hold);
  ArmMotor.setStopping(hold);
  
  ClawMotor.setVelocity(30, percent);
  ClawMotor.setVelocity(60, percent);

  // Use tank drive to control the robot.
  while (true) {
    LeftMotor.setVelocity(Controller1.Axis3.position(), percent);
    RightMotor.setVelocity(Controller1.Axis2.position(), percent);
    LeftMotor.spin(forward);
    RightMotor.spin(forward);
    
    wait(25, msec);
  }
  
}
```

## robotconfig 
``` cpp
#include "vex.h"

using namespace vex;
using signature = vision::signature;
using code = vision::code;

// A global instance of brain used for printing to the V5 Brain screen
brain  Brain;

// VEXcode device constructors
controller Controller1 = controller(primary);
motor LeftMotor = motor(PORT1, ratio18_1, false);
motor RightMotor = motor(PORT10, ratio18_1, true);
motor ArmMotor = motor(PORT8, ratio18_1, false);
motor ClawMotor = motor(PORT3, ratio18_1, false);

// VEXcode generated functions
// define variable for remote controller enable/disable
bool RemoteControlCodeEnabled = true;
// define variables used for controlling motors based on controller inputs
bool Controller1LeftShoulderControlMotorsStopped = true;
bool Controller1RightShoulderControlMotorsStopped = true;

// define a task that will handle monitoring inputs from Controller1
int rc_auto_loop_function_Controller1() {
  // process the controller input every 20 milliseconds
  // update the motors based on the input values
  while(true) {
    if(RemoteControlCodeEnabled) {
      // check the ButtonL1/ButtonL2 status to control ArmMotor
      if (Controller1.ButtonL1.pressing()) {
        ArmMotor.spin(forward);
        Controller1LeftShoulderControlMotorsStopped = false;
      } else if (Controller1.ButtonL2.pressing()) {
        ArmMotor.spin(reverse);
        Controller1LeftShoulderControlMotorsStopped = false;
      } else if (!Controller1LeftShoulderControlMotorsStopped) {
        ArmMotor.stop();
        // set the toggle so that we don't constantly tell the motor to stop when the buttons are released
        Controller1LeftShoulderControlMotorsStopped = true;
      }
      // check the ButtonR1/ButtonR2 status to control ClawMotor
      if (Controller1.ButtonR1.pressing()) {
        ClawMotor.spin(forward);
        Controller1RightShoulderControlMotorsStopped = false;
      } else if (Controller1.ButtonR2.pressing()) {
        ClawMotor.spin(reverse);
        Controller1RightShoulderControlMotorsStopped = false;
      } else if (!Controller1RightShoulderControlMotorsStopped) {
        ClawMotor.stop();
        // set the toggle so that we don't constantly tell the motor to stop when the buttons are released
        Controller1RightShoulderControlMotorsStopped = true;
      }
    }
    // wait before repeating the process
    wait(20, msec);
  }
  return 0;
}

/**
 * Used to initialize code/tasks/devices added using tools in VEXcode Pro.
 * 
 * This should be called at the start of your int main function.
 */
void vexcodeInit( void ) {
  task rc_auto_loop_task_Controller1(rc_auto_loop_function_Controller1);
}
```
