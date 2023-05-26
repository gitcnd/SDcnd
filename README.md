## SDcnd

SD library for (at least) ESP32 allowing you to specify whatever pins you want for everything instead of the defaults.

## Purpose

Use this library, instead of "SD.h" in your ESP32 sketchm when you need to hook your SD car up to different SPI pins (Like how the ESP32-Cam boards do)

## Usage

Put the "SDcnd.h" and "SDcnd.cpp" files into your sketch folder, and:-

```
#include "SDcnd.h"        // Custom lib with addressable SPI pins
#define SD_CS   13        // ESP32-Cam pins: 13,14,2,15
#define SD_SCK  14
#define SD_MISO 2
#define SD_MOSI 15
```

```
  SPIClass SPI1(HSPI);

  if(!SDCND.begin( SD_CS, SPI1, 4000000, "/sd" , 5 , false , SD_SCK, SD_MISO, SD_MOSI )){
    Serial.println("SD Card Mount Failed");
  } else {
    uint8_t cardType = SDCND.cardType();
    if(cardType == CARD_NONE){
      Serial.println("No SD card inserted");
    } else {
      Serial.print("SD Card Type: ");
      if(cardType == CARD_MMC){      Serial.println("MMC"); }
      else if(cardType == CARD_SD){  Serial.println("SDSC");}
      else if(cardType == CARD_SDHC){Serial.println("SDHC");}
      else {                         Serial.println("UNKNOWN"); }
      uint64_t cardSize = SDCND.cardSize() / (1024 * 1024);
      Serial.printf("SD Card Size: %lluMB\n", cardSize);
      writeFile(SDCND, "/hello.txt", "Hello ");
    }
  }
```

## Note

This is a clone of the ESP32 SD.h libray, simply adjusted to allow you to pass through your necessary pins into their SPI constructor.


