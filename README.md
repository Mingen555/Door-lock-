# Door-lock-hyhggu
#include <LiquidCrystal.h> //include LCD library
#include <Keypad.h> //include keypad library
#include <Servo.h>  ////include servo library
#define position_Length 5   // constant int Password_Length = 5
Servo myservo;  // create servo object to control a servo
char entry_data[position_Length];  // user door password 
char door_password[position_Length] = "A123"; //  door password pre setting
byte entry_byte_count = 0;
int buzzer=11;
int greenLED=12; // door lock open, greenLED light up
int redLED=13; // door lock close, redLED light up
const byte rows = 4; //number of the keypad rows
const byte cols = 4; // number of the keypad columns
char keyMap [rows] [cols] = { //define the symbols on the buttons of the keypad
 {'1', '2', '3', 'A'},
 {'4', '5', '6', 'B'},
 {'7', '8', '9', 'C'},
 {'*', '0', '#', 'D'}
};
byte rowPins [rows] = {9, 8, 7, 6}; // digital pins related to the row pins of the keypad
byte colPins [cols] = {5, 4, 3, 2}; // digital pins related to the column pins of the keypad
Keypad myKeypad = Keypad( makeKeymap(keyMap), rowPins, colPins, rows, cols);  // Transforms the 2d key array into a key map that the library understands.
LiquidCrystal lcd (A0, A1, A2, A3, A4, A5); // pins of the LCD. (RS, E, D4, D5, D6, D7)

const int c = 261;      // tone(pin, frequency, duration)
const int d = 294;      // these constant int are frequency for buzzer Tone function
const int e = 329;      // frequency 
const int f = 349;      // frequency 
const int g = 391;      // frequency 
const int gS = 415;     // frequency 
const int a = 440;      // frequency 
const int aS = 455;     // frequency 
const int b = 466;      // frequency 
const int cH = 523;     // frequency 
const int cSH = 554;    // frequency 
const int dH = 587;     // frequency 
const int dSH = 622;    // frequency 
const int eH = 659;     // frequency 
const int fH = 698;     // frequency 
const int fSH = 740;    // frequency 
const int gH = 784;     // frequency 
const int gSH = 830;    // frequency 
const int aH = 880;     // frequency
int counter =0;

void setup(){
  myservo.attach(10);   //// attaches the servo on pin 9 to the servo object
  myservo.write(180);
 lcd.begin(16, 2);    //  Initializes the interface to the LCD screen, and specifies the dimensions (width and height) of the display.
  lcd.setCursor(2, 0);
 lcd.print("Welcome = )");
 lcd.setCursor(0, 1);
 delay(1000);
 lcd.print("DoorLock System");
 delay(2000);
 lcd.clear();
 pinMode(redLED, OUTPUT); //set the LED as an output
 pinMode(greenLED, OUTPUT); //set the LED as an output
}
void loop(){
int beep =0; 
 lcd.setCursor(0,0);
 lcd.print("Enter Password:");
 char whichKey = myKeypad.getKey(); //define which key is pressed with getKey
 if (whichKey){
 entry_data[entry_byte_count] = whichKey; 
 lcd.setCursor(entry_byte_count,1); 
 lcd.print(entry_data[entry_byte_count]); 
 delay(300);
 entry_byte_count++; 
 }
 if(entry_byte_count == position_Length-1){   // compare  user input password with pre set password 
 lcd.clear();
 if(strcmp(entry_data, door_password)==0){    //  user input password is correct  
 lcd.setCursor(0, 0);                         //
 lcd.print("Correct! :)");                    //
  delay(1000);
lcd.setCursor(0, 1);
 lcd.print("DoorLock Opened ");
  delay(3000);
  LED(1);                                     // green led blinks
  servo_function();                           //call servo function --> servo rotate--> star war sound track--> servo rotate                                            //
 }
 else{                                        // user input password is wrong  
    lcd.setCursor(0, 0);
 lcd.print("Wrong! :(");
  delay(1000);
lcd.setCursor(0, 1);
 lcd.print("Try it again");
 delay(1000);
  LED(0);                                     // red led blinks
  buzzer_function();                          // buzzer : short beep
 }
 lcd.clear();
 clear_entry_data();                // clear user input  password
 }
}
void servo_function(){
  int pos; // variable to store the servo position
  for (pos = 180; pos >= 60; pos--) { // goes from 180 degrees to 60 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);        }               // waits 15ms for the servo to reach the position
  lcd.clear();
   lcd.setCursor(0, 0);
 lcd.print("DoorLock Closed ");
  delay(1000);
lcd.setCursor(0, 1);
 lcd.print("After soundtrack");
  sound();
for (pos = 60; pos <= 180; pos++) { // goes from 60 degrees to 180 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}
void buzzer_function(){
tone (buzzer, 450);
delay(500);
noTone(buzzer);
 
}

void  clear_entry_data(){
 while(entry_byte_count !=0){
 entry_data[entry_byte_count--] = 0; 
 }
}
void LED(int NUM){
 if(NUM){
 
 digitalWrite(greenLED, HIGH);
 delay(1500);
 digitalWrite(greenLED, LOW);
 }
 else{
 digitalWrite(redLED, HIGH);
delay(1500);
 digitalWrite(redLED, LOW);
}
}
void sound(){

  firstSection();
  //Play second section
  secondSection();
  //Variant 1
  beep(f, 250);  
  beep(gS, 500);  
  beep(f, 350);  
  beep(a, 125);
  beep(cH, 500);
  beep(a, 375);  
  beep(cH, 125);
  beep(eH, 650);

  delay(500);

  //Repeat second section
  secondSection();

  //Variant 2
  beep(f, 250);  
  beep(gS, 500);  
  beep(f, 375);  
  beep(cH, 125);
  beep(a, 500);  
  beep(f, 375);  
  beep(cH, 125);
  beep(a, 650);  
  delay(650);
}
void beep(int note, int duration)
{
  //Play tone on buzzerPin
  tone(buzzer, note, duration);

  //Play different LED depending on value of 'counter'
  if(counter % 2 == 0)                      
  {                                 // even number : green led blink
    digitalWrite(greenLED, HIGH);
    delay(duration);
    digitalWrite(greenLED, LOW);
  }else
  {
    digitalWrite(redLED, HIGH);     // odd number : red led blink
    delay(duration);
    digitalWrite(redLED, LOW);
  }

  //Stop tone on buzzerPin
  noTone(buzzer);

  delay(50);

  //Increment counter
  counter++;
}

void firstSection()
{
  beep(a, 500);
  beep(a, 500);    
  beep(a, 500);
  beep(f, 350);
  beep(cH, 150);  
  beep(a, 500);
  beep(f, 350);
  beep(cH, 150);
  beep(a, 650);
  delay(500);
  beep(eH, 500);
  beep(eH, 500);
  beep(eH, 500);  
  beep(fH, 350);
  beep(cH, 150);
  beep(gS, 500);
  beep(f, 350);
  beep(cH, 150);
  beep(a, 650);
  delay(500);
}

void secondSection()
{
  beep(aH, 500);
  beep(a, 300);
  beep(a, 150);
  beep(aH, 500);
  beep(gSH, 325);
  beep(gH, 175);
  beep(fSH, 125);
  beep(fH, 125);    
  beep(fSH, 250);
  delay(325);
  beep(aS, 250);
  beep(dSH, 500);
  beep(dH, 325);  
  beep(cSH, 175);  
  beep(cH, 125);  
  beep(b, 125);  
  beep(cH, 250);  
  delay(350);
}
