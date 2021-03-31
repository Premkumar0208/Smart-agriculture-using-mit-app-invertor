# Smart-agriculture-using-mit-app-invertor
#include &lt;Arduino.h> #include &lt;WiFi.h> #include &lt;HTTPClient.h> #include &lt;ArduinoJson.h> #include "IOXhop_FirebaseESP32.h" #include "DHTesp.h" #define FIREBASE_HOST "https://smartagriculture-60197-default-rtdb.firebaseio.com/"                 #define FIREBASE_AUTH "c2Y0xLzULYgpENzyB9BjW3WIc4jpm7SlA7Z6xIDY"                     #define WIFI_SSID "ASUS_X00TD"                                          #define WIFI_PASSWORD "zenfone7"                                DHTesp dht; int dhtPin = 17; int motor = 18; String switch1 = ""; float Temp ; float Humi ; char Temp_str[10];  char Humi_str[10];                                                                   void setup() {    Serial.begin(9600);    delay(1000);    pinMode(motor, OUTPUT);                    WiFi.begin(WIFI_SSID, WIFI_PASSWORD);                                          Serial.print("Connecting to ");    Serial.print(WIFI_SSID);    while (WiFi.status() != WL_CONNECTED) {      Serial.print(".");      delay(500);    }        Serial.println();    Serial.print("Connected to ");    Serial.println(WIFI_SSID);    Serial.print("IP Address is : ");    Serial.println(WiFi.localIP());                                                          Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);       dht.setup(dhtPin, DHTesp::DHT11);                                               }  void loop() {    TempAndHumidity newValues = dht.getTempAndHumidity();   Temp =  float(newValues.temperature);   Humi = float(newValues.humidity);   Firebase.setFloat("/SmartAgriculture/HUMIDITY",Humi);   Firebase.setFloat("SmartAgriculture/TEMPRETURE",Temp);   delay(2000);   switch1 = Firebase.getString("/SmartAgriculture/PUMP");   if(switch1 =="1"){     digitalWrite(motor,HIGH);     Serial.println("MOTOR ON");   }    if(switch1 =="0"){     digitalWrite(motor,LOW);     Serial.println("MOTOR OFF");   }        }
