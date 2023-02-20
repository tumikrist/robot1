## code 
``` cpp 
/*----------------------------------------------------------------------------*/
/*                                                                            */
/*    Module:       main.cpp                                                  */
/*    Author:       VEX                                                       */
/*    Created:      Thu Sep 26 2019                                           */
/*    Description:  V5 project                                                */
/*                                                                            */
/*   This program will have the robot move left when it senses higher         */
/*   reflectivity than the threshold (Light maximum reflectivity +            */
/*   Dark maximum reflectivity / 2) and will move to the right when it        */
/*   senses less reflectivity than the threshold.                             */
/*                                                                            */
/*   IMPORTANT: In order for this program to work correctly, the Line         */
/*   Tracker needs to be mounted facing down towards the ground, close        */
/*   to the ground.                                                           */
/*                                                                            */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// LineTrackerRight     line          H               
// LeftMotor            motor         1               
// RightMotor           motor         10              
// LineTrackerLeft      line          E               
// LineTrackerMiddle    line          F               
// BumperA              bumper        A               
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;


int Keyra() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  timer myTimer = timer();
  char left_or_right ;

  int threshold = 75;
  while (true) {
    // If the reflectivity is greater than the threshold it will move the LeftMotor forward
    /* if (LineTrackerRight.reflectivity() > threshold) {
      LeftMotor.spin(forward);
      RightMotor.stop();
    } else {
      // If the reflectivity is less than the threshold it will move the RightMotor forward
      LeftMotor.stop();
      RightMotor.spin(forward);
    } */
    
    if (LineTrackerMiddle.reflectivity() < threshold){
      RightMotor.spin(forward);
      LeftMotor.spin(forward);

    } else if(LineTrackerRight.reflectivity() < threshold){
      LeftMotor.spin(forward);
      RightMotor.stop();
      left_or_right = 'R';

    } else if(LineTrackerLeft.reflectivity() < threshold){
      LeftMotor.stop();
      RightMotor.spin(forward);
      left_or_right = 'L';

    }else {
      /* LeftMotor.stop();
      RightMotor.stop(); */
      if (left_or_right == 'L'){
        LeftMotor.spin(forward);
        RightMotor.stop();
      } else if (left_or_right == 'R'){
        LeftMotor.stop();
        RightMotor.spin(forward);
      }
    }
    wait(0.25, msec);
  }
  return 0;
}


int Skjar(){
  int threshold = 75;
  while (true){
    /* Brain.Screen.clearScreen();
    Brain.Screen.setCursor(1,1);
    Brain.Screen.print("Right : ");
    Brain.Screen.print(LineTrackerRight.reflectivity());

    Brain.Screen.newLine();
    Brain.Screen.print("Middle : ");
    Brain.Screen.print(LineTrackerMiddle.reflectivity());
    Brain.Screen.newLine();

    Brain.Screen.print("Left : ");
    Brain.Screen.print(LineTrackerLeft.reflectivity());
    Brain.Screen.newLine(); */
    // skja stærð 500 x 250 y 
    Brain.Screen.clearScreen();
    Brain.Screen.setPenColor(cyan);
    Brain.Screen.setPenWidth(10);

    if(LineTrackerMiddle.reflectivity() < threshold){
      Brain.Screen.drawLine( 250 , 75 , 250 ,175);
      Brain.Screen.drawLine( 250 , 75 , 215 , 100);
      Brain.Screen.drawLine( 250 , 75 , 285 , 100);
    }
    if(LineTrackerRight.reflectivity() < threshold){
      Brain.Screen.drawLine( 275 , 75 , 225 ,175);
      Brain.Screen.drawLine( 275 , 75 , 225 , 100);
      Brain.Screen.drawLine( 275 , 75 , 285 , 125);
    }
    if(LineTrackerLeft.reflectivity() < threshold){
      Brain.Screen.drawLine( 225 , 75 , 275 ,175);
      Brain.Screen.drawLine( 225 , 75 , 200 , 125);
      Brain.Screen.drawLine( 225 , 75 , 275 , 75);
    }
    

    wait(100, msec);
  } 
  return 0;
}

int StopTakki(){
  while (true){
    if (BumperA.pressing()){
      RightMotor.stop();
      LeftMotor.stop();
      vex::task::stopAll();
    }
  }

  return 0;
}

int main(){
  vexcodeInit();

  RightMotor.setVelocity(10,percent);
  LeftMotor.setVelocity(10,percent);
  task skjar = task(Skjar);
  task keyra = task(Keyra);
  task stoptakki = task(StopTakki);
}
```
## link a virkni
https://youtube.com/shorts/YdEkzluxOxM
