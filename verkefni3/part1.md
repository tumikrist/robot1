## kóði 
``` cpp 
/*----------------------------------------------------------------------------*/
/*                                                                            */
/*    Module:       main.cpp                                                  */
/*    Author:       VEX                                                       */
/*    Created:      Tue Oct 01 2019                                           */
/*    Description:  Detecting Distances                                       */
/*                  This program will use the Range Finder to stop a robot    */
/*                  300 mm away from a wall.                                  */
/*                                                                            */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// Drivetrain           drivetrain    1, 10           
// RangeFinderC         sonar         C, D            
// BumperA              bumper        A               
// LightB               light         B               
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;
controller Controller1 = controller(primary);
motor ArmMotor = motor(PORT8, ratio18_1, false);
timer myTimer;


float heild;

int timerFall(timer temp){
  /* timer minusTimer;
  return myTimer.value() -= minusTimer.value(); */
  heild += temp.value();
  return 0;
}

int checkaLengd(){
  float allt = myTimer.value() - heild;
  Brain.Screen.print(allt/3.22);
  Brain.Screen.print("metrar keyrdirn ");
  Brain.Screen.newLine();
  Brain.Screen.print("timer heild ");
  Brain.Screen.print(myTimer.value());
  Brain.Screen.newLine();
  Brain.Screen.print("stop heild ");
  Brain.Screen.print(heild);
  return 0;
}

bool check;
timer temp1;
// main fallið
int mainTask(){
  //timer temp1;
  // The robot will stop driving when the Range Finder is less than 300 mm away
  // from an object.
  //bool check;

  while(true){
    if (RangeFinderC.distance(mm) < 550) {
      Drivetrain.turn(right);
      check = true;

    } else if(RangeFinderC.distance(mm) >= 550){
      if(check == true){
        timerFall(temp1);
      }
      temp1.reset();
      Drivetrain.drive(forward);
      check=false;
    }
  }
  return 0;
}


// task fyrir stoptakkan sem stoppar robotinn ef takinn er ýtur á
int stopTakki(){
  
  while(true){
    if(Controller1.ButtonL1.pressing() || BumperA.pressing()){
      checkaLengd();
      Drivetrain.stop();
      vex::task::stopAll(); 
    }
  }
  return 0;
}

// fyrir light sensor til að hann stoppi þegar það er ekki ljós
int lightSensor(){
  timer temp;
  while(true){
    /* Brain.Screen.print(LightB.brightness());
    Brain.Screen.clearLine() */;

    if(LightB.brightness() < 35){
      /* Brain.Screen.newLine();
      Brain.Screen.print("stop"); */
      
      vex::task::suspend(mainTask);
      Drivetrain.stop();

    } else if(LightB.brightness() > 30){
      timerFall(temp);
      vex::task::resume(mainTask);
      temp.reset();
    }
  }
  return 0;
}

// á 30 per keyrir hann 1 meter á 3.22 sec

// main fall sem keyrir hinn taskin
int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();

  Drivetrain.setDriveVelocity(30,percent);

  ArmMotor.setStopping(hold);
  ArmMotor.stop();

  // Wait 1 second before driving forward.
  task myTask = task(mainTask);
  task stoptakki = task(stopTakki);
  task lightsensor = task(lightSensor);
  //task checkalengd = task(checkaLengd);
}

```

## robot config skra
``` cpp 
#include "vex.h"

using namespace vex;
using signature = vision::signature;
using code = vision::code;

// A global instance of brain used for printing to the V5 Brain screen
brain  Brain;

// VEXcode device constructors
motor LeftDriveSmart = motor(PORT1, ratio18_1, false);
motor RightDriveSmart = motor(PORT10, ratio18_1, true);
drivetrain Drivetrain = drivetrain(LeftDriveSmart, RightDriveSmart, 319.19, 295, 40, mm, 1);
sonar RangeFinderC = sonar(Brain.ThreeWirePort.C);
bumper BumperA = bumper(Brain.ThreeWirePort.A);
light LightB = light(Brain.ThreeWirePort.B);

// VEXcode generated functions



/**
 * Used to initialize code/tasks/devices added using tools in VEXcode Pro.
 * 
 * This should be called at the start of your int main function.
 */
void vexcodeInit( void ) {
  // nothing to initialize
}
```
