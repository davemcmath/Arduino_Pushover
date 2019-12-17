#include <ESP8266WiFi.h>
//  WiFi
WiFiClient client;
const char* ssid     = "<ssid>";
const char* password = "<pass>";
// Pushover settings
char pushoversite[] = "api.pushover.net";
String Token = "<app>";
String User  = "<user>";
int length;



void setup() {
  //start serial connection
  Serial.begin(9600);
  //configure pin 2 as an input and enable the internal pull-up resistor
  pinMode(2, INPUT_PULLUP);
  pinMode(13, OUTPUT);
// WiFi setup
  Serial.print("Connecting to WiFi ");
  Serial.println(ssid);
  WiFi.mode(WIFI_STA); //Client mode only
  WiFi.setOutputPower(20.5); //Limiting output power (0 - 20.5)
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print("_");
  }
  Serial.println("");
  Serial.print(ssid);
  Serial.println(" successfully connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
  delay(200);

}

void loop() {
  //read the pushbutton value into a variable
  int sensorVal = digitalRead(2);
  //print out the value of the pushbutton
  // Serial.println(sensorVal);
  // Keep in mind the pull-up means the pushbutton's logic is inverted. It goes
  // HIGH when it's open, and LOW when it's pressed. Turn on pin 13 when the
  // button's pressed, and off when it's not:
  if (sensorVal == HIGH) {
    digitalWrite(13, LOW);
    } else {
    digitalWrite(13, HIGH);
    Serial.println("Sensor Closed");
    pushover("Driveway Sensor Activated");
    delay(30000);
  }
}

byte pushover(char *pushovermessage) {
 
  String Msg = pushovermessage;
  length = 81 + Msg.length();
    if (client.connect(pushoversite,80)) {
      Serial.println("Sending message…");
      client.println("POST /1/messages.json HTTP/1.1");
      client.println("Host: api.pushover.net");
      client.println("Connection: close\r\nContent-Type: application/x-www-form-urlencoded");
      client.print("Content-Length: ");
      client.print(length);
      client.println("\r\n");
      client.print("token="+Token+"&user="+User+"&message="+Msg);
      while(client.connected()) {
        while(client.available()) {
          char ch = client.read();
          Serial.write(ch);
        }
      }
      client.stop();
      delay(100);
    }
}
