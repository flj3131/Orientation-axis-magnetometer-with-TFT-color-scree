//1-A1324(X) -> analogRead(A1) / 2-A1324(Y) -> analogRead(A2) / 3-A1324(Z) -> analogRead(A3)
  
  // 1.44" TFT(redboard)
  // Vin VCC -> 5V
  // GND -> GND
  // SCK -> Pin 13, SCK
  // SI SDA -> Pin 11, MOSI 
  // TCS CS -> Pin 10
  // RST -> Pin 9
  // D/C A0 -> Pin 8 
  // LED -> 5V
 

  
#include <Adafruit_GFX.h>    // Core graphics library
#include <Adafruit_ST7735.h> // Hardware-specific library for ST7735
#include <SPI.h>

// Color definitions
    #define BLACK    0x0000
    #define BLUE     0x001F
    #define RED      0xF800
    #define GREEN    0x07E0
    #define CYAN     0x07FF
    #define MAGENTA  0xF81F
    #define YELLOW   0xFFE0 
    #define WHITE    0xFFFF

//Option 2, Arduino Uno or Arduino pro mini
       #define TFT_CS 10
       #define TFT_RST 9
       #define TFT_DC 8    
       #define TFT_MOSI 11
       #define TFT_SCLK 13

   Adafruit_ST7735 tft = Adafruit_ST7735(TFT_CS, TFT_DC, TFT_RST);  //Adafruit_ST7735.h 

//Shift the picture up by voffset so that it fits on the screen better.
#define voffset  10
//Zoom is the zoom factor. Use 1 for no zoom. Use 2, 3, or higher for zoom.
#define zoom 1
 
//Use Vcc=3.3V for the sensors with the MKR1010. 
    //#define vcc 3.3
//Use Vcc=5.0V for the sensors with the UNO.
  #define vcc 5.0     //
 
void setup() {

  tft.initR(INITR_144GREENTAB);    // 1.44" TFT
  delay(1000);  
}

void loop() {
  
  tft.fillScreen(BLACK);
  drawAxes3d();
  drawArrow3d(analogRead(A1), analogRead(A2), analogRead(A3) );
  delay(1000); //Lower this for faster refresh.
}


void drawAxes3d()
{
  
    uint16_t startxx,startyy,stopxx,stopyy;
    startxx= isometricxx(128, 0, 0);
    startyy=isometricyy(128,0,0, voffset);
    stopxx=64;
    stopyy=64+voffset;
    tft.drawLine(startxx,startyy, stopxx,stopyy, RED);
    startxx= isometricxx(0,128, 0);
    startyy=isometricyy(0,128,0,voffset);
    stopxx=64;
    stopyy=64+voffset;
    tft.drawLine(startxx,startyy, stopxx,stopyy, RED);
    startxx= isometricxx(0,0,128);
    startyy=isometricyy(0,0,128, voffset);
    stopxx=64;
    stopyy=64+voffset;
    tft.drawLine(startxx,startyy, stopxx,stopyy, RED);
    tft.drawChar(108, 54, 'x', RED, BLACK, 1);
    tft.drawChar(67, 10, 'y', RED, BLACK,1);
    tft.drawChar(20, 54, 'z', RED, BLACK,1);


}


void drawArrow3d(int inxx, int inyy, int inzz)
{
  //Inputs inxx, inyy and inzz range 0 to 1023 
  //which correspond to voltages 0 to 3.3V or 0 to 5V.
  //The vector should be centered.
  int onexx,oneyy,onezz;
  int ppxx,ppyy,ppzz;
  int qqxx,qqyy,qqzz;

  //Find the endpoints of the arrow.
  
  //zoom is a zoom factor defined at the top of the program.
  //onexx, oneyy, and onezz range -128 to 128.
  onexx=zoom*(inxx-512)/4;
  oneyy=zoom*(inyy-512)/4;
  onezz=zoom*(inzz-512)/4;

  //ppxx and ppyy are the start of the arrow, and qqxx and qqyy are the end.
  ppxx=isometricxx(onexx,oneyy,onezz);
  ppyy=isometricyy(onexx,oneyy,onezz,voffset);
  qqxx=isometricxx(-1*onexx,-1*oneyy,-1*onezz);
  qqyy=isometricyy(-1*onexx,-1*oneyy,-1*onezz,voffset);

  //Draw the arrow with a circle for the tip.
  tft.drawLine(ppxx,ppyy,qqxx,qqyy, GREEN);
  tft.drawCircle(ppxx,ppyy, 3, BLUE);

  
  //Now print the voltage on the bottom of the screen
  String volts;
  double vxx,vyy,vzz;
  //Convert from the ADC value to a voltage;
  vxx=inxx*vcc/1024.0;
  vyy=inyy*vcc/1024.0;
  vzz=inzz*vcc/1024.0;
  tft.setTextColor(MAGENTA);
  tft.setTextSize(1);
  tft.setTextWrap(true);
  tft.setCursor(5,100);
  volts="x=";
  volts=volts+vxx;
  volts=volts+"V";
  tft.println(volts);
  tft.setCursor(5,110);
  volts="y=";
  volts=volts+vyy;
  volts=volts+"V";
  tft.println(volts);
  tft.setCursor(5,120);
  volts="z=";
  volts=volts+vzz;
  volts=volts+"V";
  tft.println(volts);

}



int isometricxx(double inxx, double inyy, double inzz)
{
   double outxx;
   int outpixelxx;
   outxx=(inxx-inzz)/sqrt(2);
   outpixelxx=round((outxx/2)+64);
   return outpixelxx;
}
    


int isometricyy(double inxx, double inyy, double inzz, int offset)
{ 
   double outyy;
   int outpixelyy;
   outyy=-(inxx+2*inyy+inzz)/sqrt(6);
   outpixelyy=round((outyy/2)+64+voffset);
   return outpixelyy;
}

