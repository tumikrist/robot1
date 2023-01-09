"""int main() {
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
}"""
