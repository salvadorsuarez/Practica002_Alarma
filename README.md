# UNIDAD_2
# Proyecto "Sistema Notificador de Llegada de Individuos"
>Creado por: Diego Salvador Suárez Quijas

>Contacto: d.salvador0604@gmail.com

## Alarma con Arduino y un LCD desplegar.
- Mensaje (Error y Acceso)

El usuaruario debera de ingresar el PASSWORD correcto, para poder ingresar.

## Problema a Resolver.

Desarrolle una alarma con arduino para el acceso a una puerta. Cuando la puerta se abra, la alarma debería sonar.
Requisito indispensable:
Reducir el consumo de energía al mínimo. Mientras la alarma no esté sonando el arduino deberá consumir la mínima cantidad de energía.
Cuando la puerta se cierre esta deberá de dejar de sonar y regresar al estado de bajo consumo energético.

Programas:
- Arduino IDE

Materiales:
- Protoboard
- Arudino UNO
- Potenciometro (5k)
- Pantalla LCD.
- Cables
- Resistencia 220 ohmios

Librerias:
- keypad.h
- LiquidCrystal_I2C.h
- Password.h

**************************** CÓDIGO IMPLEMENTADO Y COMENTADO ****************************************

#include <Password.h> //Incluimos la libreria Password
#include <Keypad.h> //Incluimos la libreria Keypad
#include <LiquidCrystal.h>  //Incluimos la libreria LiquidCrystal
 
Password password = Password("13273");  //Definimos el Password, puede ser modificada e incluir letras (MAX 5 CARACTERES).
int dlugosc = 5;                        //Largo del Password
 
LiquidCrystal lcd(A0, A1, A2, A3, A4, A5); //Definimos los pines del LCD
 
int buzzer = 10; //Creamos las Variables de salida
int ledRed = 12; 
int ledGreen = 11;
 
int ilosc; //Numero de Clicks
 
const byte ROWS = 4; // Cuatro Filas
const byte COLS = 4; // Cuatro Columnas

// Definimos el Keymap
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = { 9,8,7,6 };// Conectar los keypads ROW1, ROW2, ROW3 y ROW4 a esos Pines de Arduino.
byte colPins[COLS] = { 5,4,3,2, };// Conectar los keypads COL1, COL2, COL3 y COL4 a esos Pines de Arduino.
 
Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );
 
void setup()
{
  Serial.begin(9600);
  keypad.addEventListener(keypadEvent);  
  pinMode(ledRed, OUTPUT);  
  pinMode(ledGreen, OUTPUT);
  pinMode(buzzer, OUTPUT);
 
  digitalWrite(ledRed, HIGH);
  digitalWrite(ledGreen, LOW);
 
  lcd.begin(16, 2);
 
  lcd.setCursor(0,0);
  lcd.print("  *Bienvenido*");
  lcd.setCursor(0,1);
  lcd.print("FAVOR ENTRE PIN");
}
 
void loop()
{
  keypad.getKey();
}
void keypadEvent(KeypadEvent eKey)
{
  switch (keypad.getState())
  {
    case PRESSED:
   
int i;
for( i = 1; i <= 1; i++ )
{
  digitalWrite(buzzer, HIGH);  
  delay(200);            
  digitalWrite(buzzer, LOW);  
  delay(100);      
}    
 
Serial.print("Pressed: ");
Serial.println(eKey);
 
switch (eKey)
{
/*
case '#':
break;
 
case '*':
break;
*/
 
default:
ilosc=ilosc+1;
password.append(eKey);
}
//Serial.println(ilosc);
 
if(ilosc == 1)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("*_");
}
if(ilosc == 2)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("**_");
}
if(ilosc == 3)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("***_");
}
if(ilosc == 4)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("****_");
}
if(ilosc == 5)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("*****_");
}
if(ilosc == 6)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("******_");
}
if(ilosc == 7)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("*******_");
}
if(ilosc == 8)
{
lcd.clear();
lcd.setCursor(1,0);
lcd.print("   < PIN >");
lcd.setCursor(0,1);
lcd.print("********");
}
 
if(ilosc == dlugosc)
{
delay(250);
checkPassword();
ilosc = 0;
}
}
}
 
void checkPassword()
{
  if (password.evaluate())
  {
int i;
for( i = 1; i <= 3; i++ )
{
  digitalWrite(buzzer, HIGH);  
  delay(120);            
  digitalWrite(buzzer, LOW);  
  delay(70);      
}    
    ilosc = 0;
    password.reset();
    
    Serial.println("Correcto");    
 
    digitalWrite(ledRed, LOW);
    digitalWrite(ledGreen, HIGH);
 
    lcd.clear();
    lcd.setCursor(0,1);
    lcd.print("<<PIN CORRECTO>>");    
 
    delay(2000);
    digitalWrite(ledGreen, LOW);
    digitalWrite(ledRed, HIGH);    
       
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("  *Bienvenido*");
    lcd.setCursor(0,1);
    lcd.print("FAVOR ENTRE PIN");   
 
  }  
  else  
  {
int i;
for( i = 1; i <= 1; i++ )
{
  digitalWrite(buzzer, HIGH);  
  delay(300);            
  digitalWrite(buzzer, LOW);  
  delay(100);      
}  
    ilosc = 0;  
    password.reset();
 
    Serial.println("Error");
 
    digitalWrite(ledGreen, LOW);
    digitalWrite(ledRed, HIGH);    
             
    lcd.clear();
    lcd.setCursor(0,1);
    lcd.print("<<PIN ERRONEO>>");
    delay(2000);
   
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("  *Bienvenido*");
    lcd.setCursor(0,1);
    lcd.print("FAVOR ENTRE PIN");    
  }
}
## Referencia.
https://www.youtube.com/watch?v=Gmm_gRrrEG8&t=331s
https://www.prometec.net/control-acceso-clave/
http://revistas.utp.ac.pa/index.php/ric/article/view/1245/html
http://arduinosembebidos.blogspot.com/2012/06/proyecto-final-sistema-de-seguridad.html
https://github.com/mapacheverdugo/control-de-acceso
https://santiagorss.wordpress.com/

