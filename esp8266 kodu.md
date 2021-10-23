#include <ESP8266WiFi.h>
#include <espnow.h>

//master ile eşleşecek struct yapıda int tipinde değişken tanımlama
typedef struct test_struct {
  int x;
  int y;
} test_struct;

test_struct myData;

//esp now üzerinden veri alındığında çağırılacak fonksiyon
void OnDataRecv(uint8_t * mac, uint8_t *incomingData, uint8_t len) {
  memcpy(&myData, incomingData, sizeof(myData));
  // gelen veri kaydeden komut
}
 
void setup() {
  Serial.begin(115200); //seri haberleşme başlangıcı

  pinMode(D1, OUTPUT);// d çıkış olarak ayarlandı
  
  //wifi istasyonu olarak ayarlandı
  WiFi.mode(WIFI_STA);

  //esp now başlatıldı
  if (esp_now_init() != 0) {
    Serial.println("Error initializing ESP-NOW");
    return;
  }
  
  // esp8266lar slave moduna geçirildi ve
  // OnDataRecv tanımlandı
  esp_now_set_self_role(ESP_NOW_ROLE_SLAVE);
  esp_now_register_recv_cb(OnDataRecv);
}
 
void loop() {
  int a, b;
Serial.println(myData.x);
Serial.println(myData.y);
Serial.println("x");

b=myData.y;
Serial.println(b);
if(b==0){ // mauel mod sorgusu
  if(myData.x == 1){ //oto mod sorgusu
  a= analogRead(A0);
a=map(a, 0, 1023, 0, 255);
a=255-a;
analogWrite(D1, a);
}
}else{
 b=map(b,0,2900,255,60);
analogWrite(D1, b);
}
}
