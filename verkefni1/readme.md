```cpp
int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();
  int dist = 500;

  for(int i = 0; i <= 4 ; i++){  
    Drivetrain.driveFor(forward, dist, mm); 
    vex::task::sleep(500);
    Drivetrain.driveFor(reverse, dist, mm);
    vex::task::sleep(500);
    dist += 500;
  }
}
```


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
motor ClawMotor = motor(PORT3, ratio18_1, false);
motor ArmMotor = motor(PORT8, ratio18_1, false);

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
video of robot 
https://youtu.be/3EHLHCeqCrs
