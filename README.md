#define signalToRelayPin 8
#define sensorPin 7

int lastSoundValue;
int soundValue;
long lastNoiseTime = 0;
long currentNoiseTime = 0;
long lastLightChange = 0;
int relayStatus = HIGH;

void setup() {
  pinMode(sensorPin, INPUT);
  pinMode(signalToRelayPin, OUTPUT);
  digitalWrite(signalToRelayPin, HIGH);
}
void loop() {
  soundValue = digitalRead(sensorPin);
  currentNoiseTime = millis();

  if (soundValue == 1) { 

    if (
      (currentNoiseTime > lastNoiseTime + 200)&& 
      (lastSoundValue == 0)&& 
      (currentNoiseTime < lastNoiseTime + 800)&&
      (currentNoiseTime > lastLightChange + 1000) 
    ) {

      relayStatus = !relayStatus;
      digitalWrite(signalToRelayPin, relayStatus);
      lastLightChange = currentNoiseTime;
     }

     lastNoiseTime = currentNoiseTime;
  }

  lastSoundValue = soundValue;
}
