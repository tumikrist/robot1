## kóði
``` cpp

int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!

  
  vexcodeInit();

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
}
```

## robot config skrá
```cpp
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
