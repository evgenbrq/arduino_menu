#include <LiquidCrystal_I2C.h>
#include <Key.h>
#include <Keypad.h>
//#include <LiquidCrystal.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// initialize the library by associating any needed LCD interface pin
//const int rs = 1, en = 2, d4 = 3, d5 = 4, d6 = 5, d7 = 6;
//LiquidCrystal lcd(rs, en, d4, d5, d6, d7); //инициализация LCD дисплея

const byte ROWS = 4;
const byte COLS = 3;

char keys[ROWS][COLS] =
{
  {'3', '2', '1'},
  {'6', '5', '4'},
  {'9', '8', '7'},
  {'#', '0', '*'}
};

byte rowPins[ROWS] = {7, 8, 9, 10};
byte colPins[COLS] = {13, 12, 11};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS); // Инициализация Клавиатуры

struct ListMenu
{
  uint8_t parent; //родитель
  uint8_t count; //кол-во элементов в массиве
  uint8_t unique; //уникальный номер элемента
  char name[20]; //имя элемента
};

void PrintMenu(ListMenu menu[], int num); //Даннная функция производит манипуляции с пунктами меню
void Emulation_arduino(ListMenu menu[]); //Данная функция печатает пункты меню на экране

ListMenu menu[13]; //Инициализация массива структур для меню


void setup() {
  //Serial.begin(9600);
  //lcd.begin(16, 2);
  lcd.init();
  lcd.backlight();
  lcd.noBlink();

  menu[0].parent = -1;
  menu[0].count = 4;
  menu[0].unique = 0;
  strcpy(menu[0].name, "Меню");

  menu[1].parent = 0;
  menu[1].count = 2;
  menu[1].unique = 1;
  strcpy(menu[1].name, "Edit data");

  menu[2].parent = 0;
  menu[2].count = 1;
  menu[2].unique = 2;
  strcpy(menu[2].name, "LCD");

  menu[3].parent = 0;
  menu[3].count = 2;
  menu[3].unique = 3;
  strcpy(menu[3].name, "Programm");

  menu[4].parent = 0;
  menu[4].count = 3;
  menu[4].unique = 4;
  strcpy(menu[4].name, "Testings");

  menu[5].parent = 1;
  menu[5].count = 0;
  menu[5].unique = 5;
  strcpy(menu[5].name, "Vlazjnost");

  menu[6].parent = 1;
  menu[6].count = 0;
  menu[6].unique = 6;
  strcpy(menu[6].name, "Davlenie");

  menu[7].parent = 2;
  menu[7].count = 0;
  menu[7].unique = 7;
  strcpy(menu[7].name, "Yarkost");

  menu[8].parent = 3;
  menu[8].count = 0;
  menu[8].unique = 8;
  strcpy(menu[8].name, "Pr1");

  menu[9].parent = 3;
  menu[9].count = 0;
  menu[9].unique = 9;
  strcpy(menu[9].name, "Pr2");

  menu[10].parent = 4;
  menu[10].count = 0;
  menu[10].unique = 10;
  strcpy(menu[10].name, "Dt1");

  menu[11].parent = 4;
  menu[11].count = 0;
  menu[11].unique = 11;
  strcpy(menu[11].name, "Dt2");

  menu[12].parent = 4;
  menu[12].count = 0;
  menu[12].unique = 12;
  strcpy(menu[12].name, "Dt3");

  lcd.clear();
  lcd.setCursor(3, 0);          // Установить курсор на первыю строку
  lcd.print("Project SS");    // Вывести текст
  lcd.setCursor(4, 1);          // Установить курсор на вторую строку
  lcd.print("Teplica");
  delay(2000);
}

void loop() {
  char key = keypad.getKey();
  if (key == '*')
  {
    PrintMenu(-1, 4, 0);
  }\
  delay(15);
  lcd.clear();
  lcd.setCursor(0, 0);          // Установить курсор на первыю строку
  lcd.print("Tmp:");    // Вывести текст
  lcd.setCursor(5, 0);          // Установить курсор на вторую строку
  lcd.print("15");

  switch (key) {
    case '1':
      lcd.noBacklight();
      break;
    case '3':
      lcd.backlight();
      break;
  }
}

void PrintMenu(int parent, int count, int unique)
{
  char key = keypad.getKey();
  int num_str = 0;//мы разбиваем кол-во элементов в подменю по 2 символа, это номер парных элементов
  int start; //уникальный номер элемента с которого начинается уровень меню
  int num = 0; //номер выбранного элемента

  for (int i = 0; i < 13; i++) {
    if (menu[i].parent == unique) {
      start = menu[i].unique;
      num_str = menu[i].unique;
      break;
    }
  }

  while (key != '#')
  {
    key = keypad.getKey();
    if (start >= num_str + count)
    {
      num = 0;
      start = num_str;
    }
    if (num_str > start)
    {
      start = num_str;
    }
    switch (key)
    {
      case '2':
        if (num <= 0) {
          if (num_str == start && count % 2 == 0) {
            num = 1;
            start = start + count - 2;
            break;
          }
          else if (num_str == start && count % 2 == 1) {
            num = 0;
            start = start + count - 1;
            break;
          } else {
            num = 1;
            start -= 2;
          }
        }
        else {
          num--;
        }
        break;
      case '8':
        if (num >= 1) {
          if (num_str + count - 1 == menu[start + 1].unique) { //проверяем последние ли это элементы для вывода на дисплей
            num = 0;                                       //если начальная позиция + кол-во элементов за минусом один, т.к экран на две строки и при переходе мы
            start = num_str;                               //прибавляем к позициям число 2 и все это сравниваем со след позицией, т.е последней в этом списке
            break;
          }
          num = 0;
          start += 2;
        }
        else if (num == 0 && menu[start].parent != menu[start + 1].parent)
        {
          num = 0;
          start = num_str;
          break;
        } else
        {
          num++;
        }
        break;
      case '5':
        PrintMenu(menu[start + num].parent, menu[start + num].count, menu[start + num].unique);
        break;
    }
    Print(num, start);
    delay(10);
  }
}

void Print(int num, int s) {
  lcd.clear();
  switch (num) {
    case 0:
      if (menu[s].parent != menu[s + 1].parent) {
        lcd.print(">");
        lcd.setCursor(2, 0);          // Установить курсор на первыю строку
        lcd.print(menu[s].name);    // Вывести текст
        break;
      }
      lcd.setCursor(0, 0);          // Установить курсор на первыю строку
      lcd.print(">");
      lcd.setCursor(2, 0);          // Установить курсор на первыю строку
      lcd.print(menu[s].name);    // Вывести текст
      lcd.setCursor(0, 1);          // Установить курсор на вторую строку
      lcd.print(menu[s + 1].name);
      break;
    case 1:
      lcd.setCursor(0, 0);          // Установить курсор на первыю строку
      lcd.print(menu[s].name);    // Вывести текст
      lcd.setCursor(0, 1);          // Установить курсор на первыю строку
      lcd.print(">");
      lcd.setCursor(1, 1);          // Установить курсор на вторую строку
      lcd.print(menu[s + 1].name);
      break;
  }
}
