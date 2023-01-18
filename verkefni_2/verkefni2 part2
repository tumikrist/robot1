## link á youtube af verkefni
https://youtube.com/shorts/RYnixWkvdH8?feature=share

## koði
``` cpp
/*----------------------------------------------------------------------------*/
/*                                                                            */
/*    Module:       main.cpp                                                  */
/*    Author:       VEX                                                       */
/*    Created:      Wed Sep 25 2019                                           */
/*    Description:  Moving Forward (mm)                                       */
/*                                                                            */
/*    This Program drives the robot forward for 150 millimeters.              */
/*                                                                            */
/*                                                                            */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// Drivetrain           drivetrain    1, 10           
// BumperA              bumper        A               
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;

controller Controller1 = controller(primary);


int mainTask(){
  // 1 = hægri
  // 2 = vinstri
  int beyggjur[] = {1,2,2,1,1,2,1,1,2,1,1,2,2,1};

  Drivetrain.driveFor(forward, 500, mm);

  for(int i = 0 ; i <= sizeof(beyggjur)/sizeof(int) ;i++){
    vex::task::sleep(200);

    if (beyggjur[i] == 1 ){
      Drivetrain.turnFor(right,90,degrees);
    } else if(beyggjur[i] == 2) {
      Drivetrain.turnFor(left,90,degrees);
    }
    
    vex::task::sleep(200);
    Drivetrain.driveFor(forward, 500, mm);

  }
  Drivetrain.stop();

  return 0;
}


int stopTakki(){
  while(true){
    if(Controller1.ButtonL1.pressing() || BumperA.pressing()){
      vex::task::stopAll();
    }
  }
  return 0;
}


int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();

  task myTask = task(mainTask);
  task stoptakki = task(stopTakki);
}
```
