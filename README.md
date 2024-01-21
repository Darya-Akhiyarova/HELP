# HELP

```C++
#include <Keypad.h>
#include <GyverGFX.h>
#include <RunningGFX.h>
#include <GyverMAX7219.h>
MAX7219 <1, 1, 10, 9, 11> mtrx;
bool game = true;
char keys[3][3] = {
  {'0', 'U',  '0'},
  {'L', '0',  'R'},
  {'0', 'D',  '0'},
};
byte rowPins[3] = {3, 4, 5}; //connect to the row pinouts of the keypad
byte colPins[3] = {6, 7, 8}; //connect to the column pinouts of the keypad

Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, 3, 3);
byte player[2] = {0, 7};
byte maze[8][8] = {
  {2, 1, 1, 1, 0, 0, 0, 0},
  {0, 0, 0, 1, 0, 1, 1, 0},
  {1, 1, 0, 1, 0, 1, 1, 0},
  {1, 0, 0, 1, 0, 1, 1, 0},
  {1, 0, 1, 1, 0, 0, 1, 0},
  {1, 0, 0, 0, 1, 1, 1, 0},
  {1, 1, 1, 0, 0, 0, 0, 0},
  {1, 1, 1, 1, 1, 0, 1, 1}
};
byte visual[8][8] = {
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0},
  {0, 0, 0, 0, 0, 0, 0, 0}
};

void setup() {
pinMode(2, INPUT);
pinMode(3, INPUT);
pinMode(4, INPUT);
pinMode(5, INPUT);
mtrx.begin();
Serial.begin(9600);
  // put your setup code here, to run once:

}

void loop() {
char key = keypad.getKey();
if (game){
//рисование
  //лабиринт
  for(byte i; i <= 7; i ++){
    for(byte j; j < 8; j ++){
      if (visual[i][j] == 1){
        mtrx.dot(i, j);
      }
    }
  }
  //игрок
  mtrx.dot(player[1],player[0]);
  mtrx.update();

//ходьба
  if (key == 'U'){
    delay(100);
    if (player[0] > 0){
      mtrx.clear();
      player[0] -= 1;
    }
  }
  if (key == 'D'){
    delay(100);
    if (player[0] < 7){
      mtrx.clear();
      player[0] += 1;
    }
  }
  if (key == 'L'){
    delay(100);
    if (player[1] > 0){
      mtrx.clear();
      player[1] -= 1;
    }
  }
  if (key == 'R'){
    delay(100);
    mtrx.clear();
    if (player[1] < 7){
      player[1] += 1;
    }
  }
    
// проверка подения
    if (maze[player[1]][player[0]] == 1){
      visual[player[1]][player[0]] = 1;
      player[0] = 0;
      player[1] = 7;
    }

//  проверка победы
    if (maze[player[1]][player[0]] == 2){ 
    mtrx.clear();
    mtrx.line(5,5,6,6);
    mtrx.line(6,5,5,6);
    mtrx.line(1,1,2,2);
    mtrx.line(2,1,1,2);
    mtrx.line(0,4,2,6);
    mtrx.line(5,6,7,4);
    mtrx.line(3,6,4,6);
    mtrx.update();
    game = false;
    
    }
    
    delay(50);
}   
}
```
