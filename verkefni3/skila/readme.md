# 3.1
## sauðkóði
Vélmennið keyrir áfram þar til að það verður komið að vegg. ef ýtt er á takkan á vélmenninu, ef ekkert ljós kemst að ljós skynjarann slekku á vélmenninu.
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

# 3.2
## Sauðkóði
Vélmennið gerir event úr því að sjá lit og eru með breytur fyrir hvern lit. Segir síðan til um hvort hann sér hvern lit með föll fyrir hvern möguleika.
## Kóði
``` cpp
#include "vex.h"

using namespace vex;

event checkGreen = event();
event checkOrange = event();
event checkPurble = event();

void hasGreenCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(1, black);
  Brain.Screen.setCursor(1, 1);
  Vision.takeSnapshot(Vision__GREENBOX);
  if (Vision.objectCount > 0) {
    Brain.Screen.print("Þarna er græni kassinn!!!");
  } else {
    Brain.Screen.print("sé ekki græna kassann :(");
  }
}

void hasOrangeCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(2, black);
  Brain.Screen.setCursor(2, 1);
  Vision.takeSnapshot(Vision__ORANGEBOX);
  if (Vision.objectCount > 0) {
    Brain.Screen.print("Þarna er appelsínuguli kassinn!!!");
  } else {
    Brain.Screen.print("sé ekki appelsínuguli kassann :(");
  }
}

void hasPurpleCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(3, black);
  Brain.Screen.setCursor(3, 1);
  Vision.takeSnapshot(Vision__PURBLEBOX);
  if (Vision.objectCount > 0) {
    Brain.Screen.print("Þarna er fjólublái kassinn!!!");
  } else {
    Brain.Screen.print("sé ekki fjólublái kassann :(");
  }
}

int eSwitch(){
  while(true){
    if(Controller1.ButtonL1.pressing() || BumperA.pressing()){
      vex::task::stopAll();
    }
  }
  return 0;
}

int main() {
  vexcodeInit();

  checkGreen(hasGreenCallback);
  checkOrange(hasOrangeCallback);
  checkPurble(hasPurpleCallback);
  task ESwitch = task(eSwitch);
  
  while (true) {
    checkGreen.broadcastAndWait();
    checkOrange.broadcastAndWait();
    checkPurble.broadcastAndWait();
    wait(1, seconds);
  }
}
```
## Myndband
[Hér er eitthvað myndband](https://youtu.be/yDBhrvbfMdo)


# 3.3
## sauðkóði
Hér er set breytur fyrir hvort myndavélin sér lit. og mælir lengidna til kubs miðað við offsetið. og vélmennið fer nær því innan við ákveðna vegalengd. Vélmennið bakkar ef kassi er of nálægt.
## kóði
``` cpp
#include "vex.h"

using namespace vex;

event checkGreen = event();
event checkOrange = event();
event checkPurple = event();

const int center = 230;
const int dist = 5200;

void hasGreenCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(1, black);
  Brain.Screen.setCursor(1, 1);
  Vision.takeSnapshot(Vision__GREENBOX);
  if (Vision.objectCount > 0) {
    Vision.takeSnapshot(Vision__GREENBOX);
    Brain.Screen.print(Vision.objects[0].width * Vision.objects[0].height);
    if(abs(Vision.objects[0].centerX-center) < 40) {
      if(Vision.objects[0].width * Vision.objects[0].height < dist){
        Drivetrain.drive(forward);
      }else if(Vision.objects[0].width * Vision.objects[0].height - dist < 1000){
        Drivetrain.stop();
      }
      else{
        Drivetrain.drive(reverse);
      }
    }else if (Vision.objects[0].centerX < center){
      Drivetrain.turn(left);
    }else{
      Drivetrain.turn(right);

  }
  } else {
    Brain.Screen.print("sé ekki græna kassann :(");
  }
}


void hasPurpleCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(2, black);
  Brain.Screen.setCursor(2, 1);
  Vision.takeSnapshot(Vision__PURBLEBOX);
  if (Vision.objectCount > 0) {
    Vision.takeSnapshot(Vision__PURBLEBOX);
    Brain.Screen.print(Vision.objects[0].width * Vision.objects[0].height);
    if(abs(Vision.objects[0].centerX-center) < 40) {
      if(Vision.objects[0].width * Vision.objects[0].height < dist){
        Drivetrain.drive(forward);
      }else if(Vision.objects[0].width * Vision.objects[0].height - dist < 1000){
        Drivetrain.stop();
      }
      else{
        Drivetrain.drive(reverse);
      }
    }else if (Vision.objects[0].centerX < center){
      Drivetrain.turn(left);
    }else{
      Drivetrain.turn(right);

  }
  } else {
    Brain.Screen.print("sé ekki fjólubláa kassann :(");
  }
}

void hasOrangeCallback() {
  Brain.Screen.setFont(mono40);
  Brain.Screen.clearLine(3, black);
  Brain.Screen.setCursor(3, 1);
  Vision.takeSnapshot(Vision__ORANGEBOX);
  if (Vision.objectCount > 0) {
    Vision.takeSnapshot(Vision__ORANGEBOX);
    Brain.Screen.print(Vision.objects[0].width * Vision.objects[0].height);
    if(abs(Vision.objects[0].centerX-center) < 40) {
      if(Vision.objects[0].width * Vision.objects[0].height < dist){
        Drivetrain.drive(forward);
      }else if(Vision.objects[0].width * Vision.objects[0].height - dist < 1000){
        Drivetrain.stop();
      }
      else{
        Drivetrain.drive(reverse);
      }
    }else if (Vision.objects[0].centerX < center){
      Drivetrain.turn(left);
    }else{
      Drivetrain.turn(right);

  }
  } else {
    Brain.Screen.print("sé ekki appelsínugula kassann :(");
  }
}

int eSwitch(){
  while(true){
    if(Controller1.ButtonL1.pressing() || BumperA.pressing()){
      vex::task::stopAll();
    }
  }
  return 0;
}

int main() {
  vexcodeInit();

  Drivetrain.setTurnVelocity(10,percent);
  Drivetrain.setDriveVelocity(20,percent);

  checkGreen(hasGreenCallback);
  checkOrange(hasOrangeCallback);
  checkPurple(hasPurpleCallback);
  task ESwitch = task(eSwitch);
  
  while (true) {
    checkGreen.broadcastAndWait();
    checkOrange.broadcastAndWait();
    checkPurple.broadcastAndWait();
  }
}
```

## myndband
[Myndbandið liggur hér](https://www.youtube.com/watch?v=Qqsx0y4aNhw)
