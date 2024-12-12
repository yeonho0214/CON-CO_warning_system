CON-CO_warning_system
=====================

## 아두이노를 활용한 센서 및 부저 조립과 구현

### 1. MQ-9 가스센서 
#### 개요
* MQ-9 가스 센서는 다양한 가스의 농도를 감지할 수 있는 센서로, 특히 일산화탄소(CO), 메탄(CH₄), 그리고 액화석유가스(LPG)와 같은 가연성 가스에 민감합니다. 이 센서는 가스 누출 감지, 대기 질 모니터링, 산업 안전 시스템 등에 사용됩니다.
#### 12/2(월)_JETBOT과 연결 및 측정 시도
> <img src="https://github.com/user-attachments/assets/f15ed0e5-3371-40a2-96b0-ed486a6294dc" alt="Image 1" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/12600b08-ccf8-4090-a4b6-e7ea8cf2b8ab" alt="Image 2" style="width: 30%;"/>

> <img src="https://github.com/user-attachments/assets/d4c0ebcd-934b-4385-bb7f-859cee0ae6cc" alt="Image 2" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/087457fa-1963-446a-8c8b-a9a2f2d3a42c" alt="Image" style="width: 30%;"/>
> * mq 9을 블로그에 나오는데로 다 해봤는데 농도 변화가 크게 차이가 나지 않는다. (블로그 링크 : [https://cafe.naver.com/mechawiki/3521])
> * mq9 이랑 mq7의 차이는 7이 일산화탄소를 집중적으로 더 잘잡는다고 해서 7으로 교체했다.
>    * MQ-7은, CO 감지에만 초점이 맞춰져 있어서 감지 범위가 20~2000 ppm으로 넓으며, 농도가 더 높은 환경에서도 안정적으로 작동한다.
>    * MQ-9은 다양한 가스와의 반응을 고려해야 하므로, 특정 가스(CO)에 대한 민감도가 상대적으로 낮아질 수 있다.


* MQ-9 사용 코드   
<pre>
<code>
const int MQ9_AOUT_PIN = A0; // MQ-9의 AOUT 핀이 연결된 아날로그 핀
float sensorValue;           // 센서 출력 값

void setup() {
  Serial.begin(9600); // 시리얼 통신 시작
  pinMode(MQ9_AOUT_PIN, INPUT); // MQ-9 핀을 입력으로 설정
}

void loop() {
  // 아날로그 값 읽기
  sensorValue = analogRead(MQ9_AOUT_PIN);

  // 센서 값 변환 (0~1023 → 가스 농도)
  float voltage = sensorValue * (5.0 / 1023.0); // 5V 기준 전압 변환
  float co_concentration = voltage * 100;      // 임의 농도 계산 (보정 필요)

  // 시리얼 모니터에 출력
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(co_concentration);
  Serial.println(" ppm"); // ppm 단위로 출력 (보정된 농도)

  delay(1000); // 1초 간격으로 업데이트
}
</code>
</pre>


***
### 2. MQ-7 가스센서 (12/4.수)
#### 개요
* MQ-7은, MQ-7은 일산화탄소(CO) 감지에 특화된 센서로, 높은 CO 감도와 20~2000ppm의 농도를 측정할 수 있습니다. 히터 전압을 주기적으로 변경하여 작동하며, 주로 보일러실이나 자동차 배기가스 감지에 사용됩니다.
#### 12/4(수)_JETBOT과 연결 및 측정 시도
> <img src="https://github.com/user-attachments/assets/d91a8dbe-538e-4ac1-be59-bfde55e4357c" alt="Image 1" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/c4af8b61-ead2-4e2d-98b6-d1453c07f33d" alt="Image 2" style="width: 50%;"/>
> * 디스코드를 활용해 임계치 도달 시 알림이 뜨게 하기 위해서 추가로 코딩이 필요하고, 부저도 활용해볼 계획.
> * 센서의 아날로그 출력 값을 ppm 단위로 변환하기 위해, a,b 값을 찾아야 한다.
>    * 데이터 시트에 있는 CO Curve 식에 가장 일반적으로 사용되는 값인, a=100, b= -1.5을 선택하고 대입하여 사용했다.

***
### 3. 디스코드로 알림 받기, 부저 알림
#### 개요
* MQ-7 센서로 측정한 CO 농도를 일정 기준에 맞춰 상태(정상, 위험 등)를  특정한 후 디스코드로 주의하라는 알림을 사용자에게 보낸다.
* 부저를 설치해 정상 상태가 아닌 CO 농도에서 경고음이 발생하도록 설계한다.

#### 12/7(토)_실험 진행
> <img src="https://github.com/user-attachments/assets/cb513ab1-e134-4dc4-8d2e-038b23f017f9" alt="Image 1" style="width: 40%;"/>
> <img src="https://github.com/user-attachments/assets/b747a1c3-58d0-4578-9070-5692ccf27e07" alt="Image 3" style="width: 50%;"/>
> <img src="https://github.com/user-attachments/assets/b470708c-b824-4159-8042-7b94a2f51f14" alt="Image 4" style="width: 40%;"/>
> <img src="https://github.com/user-attachments/assets/fbc4c6bf-ba47-4d46-963b-7fb9b8d64b3e" alt="Image 5" style="width: 40%;"/>

> * 최종적인 실험에서 1.5g 숯 3개를 사용하였다.
> * 처음에는 200, 800, 3200 ppm (WHO 권고 기준)에서 부저 알림 및 디스코드 경고 알림을 설정하였다.
>   * 정상: ppm < 200 (부저 꺼짐)
>   * 주의: 200 <= ppm < 800 (낮은 톤(1000Hz)으로 짧게 울림)
>   * 위험: 800 <= ppm < 3200 (중간 톤(2000Hz)으로 울림)
>   * 매우 위험: 3200 <= ppm (높은 톤(3000Hz)으로 계속 울림)
> * 실험 진행 도중에 농도가 충분히 높이 올라가지 않았고, 여건상 높은 CO 농도를 측정하기 어려움이 있었다.
>    * 양초 1개/5개, 숯(작은 크기) 1개로 시도하였으나 여전히 낮은 농도에 머무름.
>    * 기준치를 변경하기로 하고, 200ppm을 0.6ppm으로, 800ppm을 5ppm로, 3200ppm 20ppm으로 바꿨다. 



<pre>
<code>
const int MQ7_AOUT_PIN = A0; // MQ-7의 AOUT 핀이 연결된 아날로그 핀
const float VCC = 5.0;       // 아두이노 공급 전압
const float RL = 10.0;       // 로드 저항 (단위: kΩ)
const float R0 = 10.0;       // 깨끗한 공기에서의 센서 저항 (단위: kΩ)
const float a = 100.0;       // 데이터시트에서 제공되는 상수 a
const float b = -1.5;        // 데이터시트에서 제공되는 상수 b

void setup() {
  Serial.begin(9600); // 시리얼 통신 시작
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // 아날로그 값 읽기
  float voltage = (sensorValue / 1023.0) * VCC; // 전압 계산
  float RS = RL * (VCC - voltage) / voltage;   // 센서 저항 계산
  float ratio = RS / R0;                       // 센서 비율 계산
  float ppm = a * pow(ratio, b);               // PPM 계산

  // 결과 출력
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(ppm);
  Serial.println(" ppm");

  delay(1000); // 1초 간격으로 업데이트
}
</code>
</pre>


<pre>
<code>
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "Your_SSID";      // Wi-Fi 이름
const char* password = "Your_PASS";  // Wi-Fi 비밀번호
const char* webhook_url = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"; // 디스코드 웹훅 URL

const int MQ7_AOUT_PIN = A0; // MQ-7의 AOUT 핀
const float VCC = 5.0;       // 아두이노 전원 공급 전압
const float RL = 10.0;       // 로드 저항 (kΩ)
const float R0 = 10.0;       // 깨끗한 공기에서의 센서 저항 (kΩ)
const float a = 100.0;       // 데이터시트 상수 a
const float b = -1.5;        // 데이터시트 상수 b
float ppm_threshold = 200.0; // 경고 임계값 (단위: ppm)

void setup() {
  Serial.begin(9600);
  pinMode(MQ7_AOUT_PIN, INPUT);

  // Wi-Fi 연결
  WiFi.begin(Water430ho_5G, Water430ho_5G);
  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nConnected to WiFi!");
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // 아날로그 값 읽기
  float voltage = (sensorValue / 1023.0) * VCC; // 전압 계산
  float RS = RL * (VCC - voltage) / voltage;   // 센서 저항 계산
  float ratio = RS / R0;                       // 센서 비율 계산
  float ppm = a * pow(ratio, b);               // PPM 계산

  // 결과 출력
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(ppm);
  Serial.println(" ppm");

  // 농도가 임계값을 초과하면 디스코드로 경고
  if (ppm > ppm_threshold) {
    sendToDiscord(ppm);
  }

  delay(1000); // 1초 간격 업데이트
}

// 디스코드로 메시지 보내는 함수
void sendToDiscord(float ppm) {
  if (WiFi.status() == WL_CONNECTED) { // Wi-Fi 연결 확인
    HTTPClient http;
    http.begin(webhook_url);
    http.addHeader("Content-Type", "application/json");

    String message = "{\"content\":\"⚠️ Warning: High CO Concentration! PPM: " + String(ppm) + "\"}";

    int httpResponseCode = http.POST(message);

    if (httpResponseCode > 0) {
      Serial.println("Message sent to Discord!");
    } else {
      Serial.print("Error sending message: ");
      Serial.println(httpResponseCode);
    }

    http.end(); // HTTP 연결 종료
  } else {
    Serial.println("WiFi not connected!");
  }
}
</code>
</pre>



<pre>
<code>
const int MQ7_AOUT_PIN = A0; // MQ-7의 아날로그 출력 핀

const float VCC = 5.0;       // Arduino 공급 전압
const float RL = 10.0;       // 로드 저항 (단위: kΩ)
const float R0 = 10.0;       // 깨끗한 공기에서 센서 저항 (단위: kΩ)
const float a = 100.0;       // 데이터시트에서 제공되는 상수 a
const float b = -1.5;        // 데이터시트에서 제공되는 상수 b

void setup() {
  Serial.begin(9600); // 시리얼 통신 시작
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // 센서 값 읽기
  float voltage = (sensorValue / 1023.0) * VCC; // 전압 계산
  float RS = RL * (VCC - voltage) / voltage;   // 센서 저항 계산
  float ratio = RS / R0;                       // 센서 비율 계산
  float ppm = a * pow(ratio, b);               // PPM 계산

  // 데이터 시리얼 포트로 전송
  Serial.println(ppm);
  delay(1000); // 1초 간격
}
</code>
</pre>

* python part
  
<pre>
<code>
import serial
import time
import requests
from datetime import datetime

# Arduino와 연결할 시리얼 포트 설정
PORT = "COM3"  # Arduino가 연결된 포트 (Windows에서는 COMx, Mac/Linux에서는 /dev/ttyUSBx)
BAUDRATE = 9600  # Arduino와 동일한 보드레이트 설정
WEBHOOK_URL = "YOUR_DISCORD_WEBHOOK_URL"  # 디스코드 웹훅 URL 입력

# CO 경고 임계값
CO_THRESHOLD = 50.0  # PPM 단위

def send_discord_alert(ppm):
    """디스코드로 경고 메시지 전송"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    color = 0xFF0000 if ppm >= CO_THRESHOLD else 0xFFFF00

    data = {
        "embeds": [{
            "title": "⚠️ CO 농도 경고 ⚠️",
            "description": f"현재 CO 농도는 {ppm:.2f} ppm 입니다.",
            "color": color,
            "fields": [
                {"name": "상태", "value": "위험 수준 도달" if ppm >= CO_THRESHOLD else "정상", "inline": True},
                {"name": "측정 시간", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO 모니터링 시스템"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] 디스코드 알림 전송 성공: {ppm:.2f} ppm")
        else:
            print(f"디스코드 알림 전송 실패: {response.status_code}")
    except Exception as e:
        print(f"디스코드 알림 오류: {str(e)}")

def main():
    try:
        # Arduino 시리얼 포트 열기
        ser = serial.Serial(PORT, BAUDRATE, timeout=1)
        print(f"Arduino 연결됨: {PORT}")
        time.sleep(2)  # Arduino 초기화 대기

        while True:
            # 시리얼 데이터 읽기
            if ser.in_waiting > 0:
                line = ser.readline().decode('utf-8').strip()
                try:
                    ppm = float(line)
                    print(f"CO 농도: {ppm:.2f} ppm")

                    # CO 농도가 임계값 초과 시 디스코드 알림 전송
                    if ppm >= CO_THRESHOLD:
                        send_discord_alert(ppm)

                except ValueError:
                    print(f"잘못된 데이터 수신: {line}")
            time.sleep(1)

    except serial.SerialException as e:
        print(f"시리얼 연결 오류: {str(e)}")
    except KeyboardInterrupt:
        print("프로그램 종료")
    finally:
        if 'ser' in locals() and ser.is_open:
            ser.close()
            print("시리얼 포트 닫힘")

if __name__ == "__main__":
    main()
</code>
</pre>

***
# 여기서부터 코드 정리
## #1 아두이노 IDE
> * MQ-7 센서가 출력하는 아날로그 값을 ppm 단위로 변환하는 내용
> * 우리가 설정한 기준치(정상, 위험 등)에 넘어섰을 때 부저가 울리도록 하는 내용
<pre>
<code>
const int MQ7_AOUT_PIN = A0;  // MQ-7 센서 아날로그 출력 핀
const int buzzerPin = 8;      // 피에조 부저 연결 핀 (디지털 핀 8)
const float VCC = 5.0;       
const float RL = 10.0;       
const float R0 = 10.0;       
const float a = 100.0;       
const float b = -1.5;        

void setup() {
  Serial.begin(9600);
  pinMode(buzzerPin, OUTPUT);
  digitalWrite(buzzerPin, LOW); // 초기 상태: 부저 OFF
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN);
  float voltage = (sensorValue / 1023.0) * VCC;
  float RS = RL * (VCC - voltage) / voltage;
  float ratio = RS / R0;
  float ppm = a * pow(ratio, b);

  // ppm 값을 시리얼로 전송
  Serial.println(ppm);

  // 상태 판단
  // 정상: ppm < 200
  // 주의: 200 ≤ ppm < 800
  // 위험: 800 ≤ ppm < 3200
  // 매우 위험: ppm ≥ 3200
  if (ppm < 200) {
    // 정상: 부저 꺼짐
    noTone(buzzerPin);
  } else if (ppm < 800) {
    // 주의: 낮은 톤(1000Hz)으로 짧게 울림
    // 1초 루프 내에서 짧게 200ms 울리고 꺼짐
    tone(buzzerPin, 1000, 200);
    delay(200);
    noTone(buzzerPin);
    // 나머지 시간 대기
  } else if (ppm < 3200) {
    // 위험: 중간 톤(2000Hz)으로 울림
    // 조금 더 길게 500ms 울리고 500ms 꺼짐 (총 1초 주기)
    tone(buzzerPin, 2000, 500);
    delay(500);
    noTone(buzzerPin);
    delay(500);
  } else {
    // 매우 위험: 높은 톤(3000Hz)으로 계속 울림
    // 여기서는 tone을 계속 주기 어렵기 때문에 다음 루프에서도 같은 상태이면 연속음에 가깝게
    tone(buzzerPin, 3000); 
    // 1초 기다린 후 다시 loop 진입 -> 사실상 계속 울림
    delay(1000);
    return; // 아래 delay(1000)로 안 내려가도록
  }

  delay(1000); // 다음 측정까지 1초 대기
}

</code>
</pre>


***
## #2 디스코드 알림 전송
> * 디스코드 알림 전송하는 내용
> * 알림 형식: '⚠️ CO {status} 상태 ⚠️', "현재 CO 농도는 {ppm:.2f} ppm 입니다." (현 농도와 상태를 알 수 있음)
<pre>
<code>
import serial
import time
import requests
from datetime import datetime
import csv
import os

# ===== 사용자 환경에 맞게 수정해야 하는 부분 =====
PORT = "/dev/ttyACM0"  # Jetson Nano에서 Arduino 포트 확인 (예: /dev/ttyACM0)
WEBHOOK_URL = "YOUR_DISCORD_WEBHOOK_URL"  # 실제 Discord Webhook URL
CSV_FILE_PATH = "/home/your_username/data/co_readings.csv"  # CSV 파일 저장 경로
# ==============================================

# 상태 기준
# 정상: ppm < 200
# 주의: 200 ≤ ppm < 800
# 위험: 800 ≤ ppm < 3200
# 매우 위험: ppm ≥ 3200

def init_csv_file(filename):
    dir_path = os.path.dirname(filename)
    if dir_path and not os.path.exists(dir_path):
        os.makedirs(dir_path, exist_ok=True)
    # 파일이 없을 경우 헤더 기록
    if not os.path.exists(filename):
        with open(filename, mode='w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(["timestamp", "ppm"])
        print(f"CSV 파일 생성 및 헤더 작성 완료: {filename}")

def append_csv_file(filename, timestamp_str, ppm_value):
    with open(filename, mode='a', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow([timestamp_str, ppm_value])

def get_co_status(ppm):
    if ppm < 200:
        return "정상", 0x00FF00  # Green
    elif ppm < 800:
        return "주의", 0xFFFF00  # Yellow
    elif ppm < 3200:
        return "위험", 0xFFA500  # Orange-ish
    else:
        return "매우 위험", 0xFF0000  # Red

def send_discord_alert(ppm):
    """CO 농도별 단계 알림 전송"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    status, color = get_co_status(ppm)
    description = f"현재 CO 농도는 {ppm:.2f} ppm 입니다."
    title = f"⚠️ CO {status} 상태 ⚠️"
    
    data = {
        "content": "@here",
        "embeds": [{
            "title": title,
            "description": description,
            "color": color,
            "fields": [
                {"name": "상태", "value": status, "inline": True},
                {"name": "측정 시간", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO 모니터링 시스템"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] 디스코드 알림 전송 성공: {ppm:.2f} ppm ({status})")
        else:
            print(f"디스코드 알림 전송 실패: {response.status_code}")
    except Exception as e:
        print(f"디스코드 알림 오류: {str(e)}")

def main():
    init_csv_file(CSV_FILE_PATH)

    try:
        ser = serial.Serial(PORT, 9600, timeout=1)
        print(f"Arduino 연결됨: {PORT}")
        time.sleep(2)  # 시리얼 초기화 대기 시간

        last_alert_status = "정상"  # 이전에 보낸 알림 상태를 추적하여 불필요한 반복 알림 방지 가능(옵션)
        
        while True:
            if ser.in_waiting > 0:
                line = ser.readline().decode('utf-8').strip()
                try:
                    ppm = float(line)
                    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                    append_csv_file(CSV_FILE_PATH, current_time, ppm)
                    print(f"CO 농도: {ppm:.2f} ppm")

                    # 상태 판단
                    status, _ = get_co_status(ppm)

                    # 정상일 때는 알림 필요 없다고 가정(원하면 조건 수정)
                    if status in ["주의", "위험", "매우 위험"]:
                        # 상태별로 조건
                        # 이전 상태와 동일한 상태라면 계속 알림 보내지 않도록 할 수도 있음.
                        # 여기서는 매번 임계 이상이면 알림 보내도록 유지.
                        send_discord_alert(ppm)

                except ValueError:
                    print(f"잘못된 데이터 수신: {line}")
            time.sleep(1)

    except serial.SerialException as e:
        print(f"시리얼 연결 오류: {str(e)}")
    except KeyboardInterrupt:
        print("프로그램 종료 요청 받음")
    finally:
        if 'ser' in locals() and ser.is_open:
            ser.close()
            print("시리얼 포트 닫힘")

if __name__ == "__main__":
    main()



PORT = "/dev/ttyUSB0"  # Jetson Nano에서 Arduino 포트 확인 (예: /dev/ttyACM0)
WEBHOOK_URL = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"  # 실제 Discord Webhook URL
CSV_FILE_PATH = "/home/dli/CO_ver2/co_readings_Beta.csv"  # CSV 파일 저장 경로

      
</code>
</pre>

* 웹후크 = [https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe]


***
## #3 CO 모니터링 시스템
* CO 농도를 실시간으로 모니터링하고 예측하며, 사용자 질의에 대한 답변과 상태 알림을 동시에 제공합니다.
> 1. OpenAI Function을 통한 사용자 질의 처리 : 사용자가 CO 농도 관련 질문을 입력하면, 적절한 답변을 제공하거나 CO 임계값에 도달할 시간을 예측한다.
>    * 'openai_chat(query)' : 필요한 경우 함수를 호출해 임계값 도달 시간을 계산한 후 답변을 생성.
>    * 'gradio_interface(query)' : OpenAI Function의 결과를 Gradio 인터페이스에서 출력하도록 연결.
>
> 2. Gradio 인터페이스 : 사용자 친화적인 웹 인터페이스를 제공하여 CO 상태 및 예측에 대한 질의응답을 처리
>    * 입력-텍스트 질의 (예: "300ppm에 도달하려면 얼마나 걸리나요?")
>    * 출력-예측된 시간 또는 상태 정보.
>    * 웹서버-Gradio가 제공하는 웹 UI를 통해 실행.
>
> 3. CO 농도 데이터 처리 및 알림
>   * 데이터 수집
>     * Arduino에서 CO 농도 데이터를 시리얼 통신을 통해 수신.
>     * 1초 간격으로 데이터를 읽어 CSV 파일에 저장하고 버퍼('ppm_buffer')에 추가.
>     * 예외 처리 : 데이터가 잘못된 형식이면 무시하고 로그에 기록.
>   * 상태 확인 및 알림
>     * 'get_co_status(ppm)' : CO 농도 상태(정상, 주의, 위험, 매우 위험)를 판단.
>     * 'send_discord_alert(ppm)' : 주의 이상의 상태에서는 Discord로 알림 전송.
>
> 4. CSV 파일 초기화 및 데이터 저장
>   * 'init_csv_file' : 지정한 파일 경로에 디렉토리와 CSV 파일을 생성, 헤더를 추가.
>   * 'append_csv_file' : 시간과 ppm 값을 CSV 파일에 추가.
>
> 5. CO 농도 예측
>   * 'add_ppm_data(ppm)' : 최근 1분간의 데이터를 관리하며 오래된 데이터를 제거.
>   * 'predict_time_to_reach_threshold(threshold)':
>     * 최근 1분간 데이터를 선형 회귀로 분석해 특정 임계값(ppm)에 도달할 시간을 예측.
>     * 농도가 증가하지 않을 경우나 데이터가 부족할 경우 적절한 메시지를 반환.
>
> 6. 멀티스레드 서버 실행 : Gradio 서버와 실시간 데이터 수집 루프를 동시에 실행하기 위해 Python의 스레드를 활용
>    * Gradio 인터페이스 실행: iface.launch를 별도 스레드에서 실행.
>    * CO 농도 데이터 수집: 메인 스레드에서 시리얼 데이터를 지속적으로 읽어 처리.
>
> 7. 예외 및 종료 처리
>    * 시리얼 연결 실패 : Arduino 연결이 실패하면 에러 메시지를 출력.
>    * 키보드 인터럽트 : 사용자가 프로그램 종료를 요청하면 안전하게 종료.
>    * 자원 해제 : 시리얼 포트를 닫아 리소스를 정리.

### <오류가 있던 graido 포함, 답변이 된 version>

<pre>
<code>
import serial
import time
import requests
from datetime import datetime
import csv
import os
import numpy as np
from openai import OpenAI
import gradio as gr
import json

# ===== 사용자 환경에 맞게 수정해야 하는 부분 =====
PORT = "/dev/ttyUSB0"  # Jetson Nano에서 Arduino 포트 확인 (예: /dev/ttyACM0)
WEBHOOK_URL = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"  # 실제 Discord Webhook URL
CSV_FILE_PATH = "/home/dli/CO_ver2/co_readings_gradio.csv"  # CSV 파일 저장 경로

# ==============================================
</code>
</pre>

<pre>
<code>
# 상태 기준
# 정상: ppm < 200
# 주의: 200 ≤ ppm < 800
# 위험: 800 ≤ ppm < 3200
# 매우 위험: ppm ≥ 3200

def init_csv_file(filename):
    dir_path = os.path.dirname(filename)
    if dir_path and not os.path.exists(dir_path):
        os.makedirs(dir_path, exist_ok=True)
    # 파일이 없을 경우 헤더 기록
    if not os.path.exists(filename):
        with open(filename, mode='w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(["timestamp", "ppm"])
        print(f"CSV 파일 생성 및 헤더 작성 완료: {filename}")

def append_csv_file(filename, timestamp_str, ppm_value):
    with open(filename, mode='a', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow([timestamp_str, ppm_value])

def get_co_status(ppm):
    if ppm < 0.6:
        return "정상", 0x00FF00  # Green
    elif ppm < 5:
        return "주의", 0xFFFF00  # Yellow
    elif ppm < 20:
        return "위험", 0xFFA500  # Orange
    else:
        return "매우 위험", 0xFF0000  # Red

def send_discord_alert(ppm):
    """CO 농도별 단계 알림 전송"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    status, color = get_co_status(ppm)
    description = f"현재 CO 농도는 {ppm:.2f} ppm 입니다."
    title = f"⚠️ CO {status} 상태 ⚠️"
    
    data = {
        "content": "@here",
        "embeds": [{
            "title": title,
            "description": description,
            "color": color,
            "fields": [
                {"name": "상태", "value": status, "inline": True},
                {"name": "측정 시간", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO 모니터링 시스템"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] 디스코드 알림 전송 성공: {ppm:.2f} ppm ({status})")
        else:
            print(f"디스코드 알림 전송 실패: {response.status_code}")
    except Exception as e:
        print(f"디스코드 알림 오류: {str(e)}")
</code>
</pre>

<pre>
<code>       
# 최근 1분간 데이터 저장용 (1초 간격 -> 60개 데이터)
ppm_buffer = []

def add_ppm_data(ppm):
    ppm_buffer.append((datetime.now(), ppm))
    # 1분보다 오래된 데이터 제거
    cutoff = datetime.now() - timedelta(seconds=60)
    # timedelta import를 위해 위 코드 상단에 from datetime import datetime, timedelta 추가해야 함
    while ppm_buffer and ppm_buffer[0][0] < cutoff:
        ppm_buffer.pop(0)

def predict_time_to_reach_threshold(threshold):
    # 최근 1분간 데이터 사용
    # ppm_buffer: [(time, ppm), ...]
    if len(ppm_buffer) < 2:
        return "데이터가 충분하지 않아 예측 불가합니다."

    # 시간축을 초 단위로 변환
    base_time = ppm_buffer[0][0]
    times = np.array([(t[0]-base_time).total_seconds() for t in ppm_buffer])
    ppms = np.array([t[1] for t in ppm_buffer])

    # 선형 회귀
    # y = m*x + c
    # np.polyfit(x, y, 1) -> (m, c)
    m, c = np.polyfit(times, ppms, 1)

    # 현재 마지막 값 기준으로 앞으로도 m(기울기)로 증가한다고 가정
    # threshold = m*x + c -> x = (threshold - c)/m
    if m <= 0:
        return "CO 농도가 증가하는 추세가 아닙니다. 환기가 급하지 않을 수 있습니다."

    x_threshold = (threshold - c)/m
    current_time_sec = (ppm_buffer[-1][0]-base_time).total_seconds()

    if x_threshold <= current_time_sec:
        return "이미 해당 임계값에 도달한 상태로 보입니다."

    delta = x_threshold - current_time_sec
    return f"{int(delta)}초 후에 CO 농도가 {threshold}ppm에 도달할 것으로 예상됩니다."
</code>
</pre>

<pre>
<code>
# OpenAI Function 정의
functions = [
    {
        "name": "predict_ventilation_time",
        "description": "지정한 임계값(ppm)에 도달하는데 걸리는 예상 시간을 반환",
        "parameters": {
            "type": "object",
            "properties": {
                "threshold": {
                    "type": "number",
                    "description": "CO 임계값(ppm)"
                }
            },
            "required": ["threshold"]
        }
    }
]

def openai_chat(query):
    messages = [
        {"role": "system", "content": "당신은 CO 농도 예측 전문가입니다. 사용자 질문에 대해 필요하면 함수를 호출하여 답해주세요."},
        {"role": "user", "content": query}
    ]
    os.environ['OPENAI_API_KEY'] = ''
    OpenAI.api_key = os.getenv("OPENAI_API_KEY")

    client = OpenAI()	
    response = client.chat.completions.create(
        model="gpt-4",
        messages=messages,
        functions=functions,
        function_call="auto"
    )

    proc_messages = []
    response_message = response.choices[0].message
    tool_calls = response_message.tool_calls

    if tool_calls:
        available_functions = {
            "predict_time_to_reach_threshold": predict_time_to_reach_threshold
        }

    # assistant의 reply를 대화 히스토리에 추가
        messages.append(response_message)

        for tool_call in tool_calls:
            function_name = tool_call.function.name
            function_to_call = available_functions.get(function_name)

            if not function_to_call:
            # 해당 함수가 없는 경우 처리
                proc_messages.append({
                    "role": "assistant",
                    "content": f"요청한 함수({function_name})를 찾을 수 없습니다."
                })
                continue

            function_args = json.loads(tool_call.function.arguments)

        # 함수 호출
        # 예: predict_time_to_reach_threshold(threshold=...)
            function_response = function_to_call(**function_args)

        # 함수 응답을 메시지에 추가
            proc_messages.append(
                {
                    "tool_call_id": tool_call.id,
                    "role": "tool",
                    "name": function_name,
                    "content": function_response,
                }
            )

    # 함수 응답 메시지를 전체 대화에 추가
        messages += proc_messages

    # 함수 호출 후 다시 모델에 요청해 최종 답변 받기
        second_response = client.chat.completions.create(
            model='gpt-4o-mini',
            messages=messages,
        )

        assistant_message = second_response.choices[0].message.content
        return assistant_message

    else:
    # 함수 호출 없이 직접 답변한 경우
        return response_message.content


def gradio_interface(query):
    answer = openai_chat(query)
    return answer



# Gradio 인터페이스
iface = gr.Interface(fn=gradio_interface,
                     inputs="text",
                     outputs="text",
                     title="CO 모니터링 & 예측 챗봇",
                     description="CO 농도 상태 및 환기가 필요한 시간 예측")

# 메인 로직 (CO 측정)
if __name__ == "__main__":
    from datetime import timedelta
    init_csv_file(CSV_FILE_PATH)

    try:
        ser = serial.Serial(PORT, 9600, timeout=1)
        print(f"Arduino 연결됨: {PORT}")
        time.sleep(1)  # 시리얼 초기화 대기 시간

        # Gradio 인터페이스 별도 스레드에서 실행
        import threading
        server_thread = threading.Thread(target=iface.launch, kwargs={"server_name":"0.0.0.0","server_port":7860}, daemon=True)
        server_thread.start()

        while True:
            if ser.in_waiting > 0:
                line = ser.readline().decode('utf-8').strip()
                try:
                    ppm = float(line)
                    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                    append_csv_file(CSV_FILE_PATH, current_time, ppm)
                    print(f"CO 농도: {ppm:.2f} ppm")

                    add_ppm_data(ppm)

                    status, _ = get_co_status(ppm)
                    if status in ["주의", "위험", "매우 위험"]:
                        send_discord_alert(ppm)

                except ValueError:
                    print(f"잘못된 데이터 수신: {line}")

            # 측정 주기 1초
            time.sleep(1)

    except serial.SerialException as e:
        print(f"시리얼 연결 오류: {str(e)}")
    except KeyboardInterrupt:
        print("프로그램 종료 요청 받음")
    finally:
        if 'ser' in locals() and ser.is_open:
            ser.close()
</code>
</pre>

***
### <최종 코드>
<pre>
<code>
import serial
import time
import requests
from datetime import datetime
import csv
import os
import numpy as np
from openai import OpenAI
import gradio as gr
import json

# ===== 사용자 환경에 맞게 수정해야 하는 부분 =====
PORT = "/dev/ttyUSB0"  # Jetson Nano에서 Arduino 포트 확인 (예: /dev/ttyACM0)
WEBHOOK_URL = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"  # 실제 Discord Webhook URL
CSV_FILE_PATH = "/home/dli/CO_ver2/co_readings_gradio.csv"  # CSV 파일 저장 경로
# ==============================================
</code>
</pre>

<pre>
<code>
# 상태 기준
# 정상: ppm < 200
# 주의: 200 ≤ ppm < 800
# 위험: 800 ≤ ppm < 3200
# 매우 위험: ppm ≥ 3200

def init_csv_file(filename):
    dir_path = os.path.dirname(filename)
    if dir_path and not os.path.exists(dir_path):
        os.makedirs(dir_path, exist_ok=True)
    # 파일이 없을 경우 헤더 기록
    if not os.path.exists(filename):
        with open(filename, mode='w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(["timestamp", "ppm"])
        print(f"CSV 파일 생성 및 헤더 작성 완료: {filename}")

def append_csv_file(filename, timestamp_str, ppm_value):
    with open(filename, mode='a', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow([timestamp_str, ppm_value])

def get_co_status(ppm):
    if ppm < 0.6:
        return "정상", 0x00FF00  # Green
    elif ppm < 5:
        return "주의", 0xFFFF00  # Yellow
    elif ppm < 20:
        return "위험", 0xFFA500  # Orange
    else:
        return "매우 위험", 0xFF0000  # Red
</code>
</pre>

<pre>
<code>
def send_discord_alert(ppm):
    """CO 농도별 단계 알림 전송"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    status, color = get_co_status(ppm)
    description = f"현재 CO 농도는 {ppm:.2f} ppm 입니다."
    title = f"⚠️ CO {status} 상태 ⚠️"
    
    data = {
        "content": "@here",
        "embeds": [{
            "title": title,
            "description": description,
            "color": color,
            "fields": [
                {"name": "상태", "value": status, "inline": True},
                {"name": "측정 시간", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO 모니터링 시스템"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] 디스코드 알림 전송 성공: {ppm:.2f} ppm ({status})")
        else:
            print(f"디스코드 알림 전송 실패: {response.status_code}")
    except Exception as e:
        print(f"디스코드 알림 오류: {str(e)}")
</code>
</pre>

<pre>
<code>
# 최근 1분간 데이터 저장용 (1초 간격 -> 60개 데이터)
ppm_buffer = []

def add_ppm_data(ppm):
    ppm_buffer.append((datetime.now(), ppm))
    # 1분보다 오래된 데이터 제거
    cutoff = datetime.now() - timedelta(seconds=60)
    # timedelta import를 위해 위 코드 상단에 from datetime import datetime, timedelta 추가해야 함
    while ppm_buffer and ppm_buffer[0][0] < cutoff:
        ppm_buffer.pop(0)

def predict_time_to_reach_threshold(threshold):
    # 최근 1분간 데이터 사용
    # ppm_buffer: [(time, ppm), ...]
    if len(ppm_buffer) < 2:
        return "데이터가 충분하지 않아 예측 불가합니다."

    # 시간축을 초 단위로 변환
    base_time = ppm_buffer[0][0]
    times = np.array([(t[0]-base_time).total_seconds() for t in ppm_buffer])
    ppms = np.array([t[1] for t in ppm_buffer])

    # 선형 회귀
    # y = m*x + c
    # np.polyfit(x, y, 1) -> (m, c)
    m, c = np.polyfit(times, ppms, 1)

    # 현재 마지막 값 기준으로 앞으로도 m(기울기)로 증가한다고 가정
    # threshold = m*x + c -> x = (threshold - c)/m
    if m <= 0:
        return "CO 농도가 증가하는 추세가 아닙니다. 환기가 급하지 않을 수 있습니다."

    x_threshold = (threshold - c)/m
    current_time_sec = (ppm_buffer[-1][0]-base_time).total_seconds()

    if x_threshold <= current_time_sec:
        return "이미 해당 임계값에 도달한 상태로 보입니다."

    delta = x_threshold - current_time_sec
    hours = delta // 3600  
    minutes = (delta % 3600) // 60  
    seconds = delta % 60  
    return f"{int(hours)}시간 {int(minutes)}분 {int(seconds)}초 후에, 즉 CO 농도가 {threshold}ppm에 도달할 것으로 예상됩니다."
</code>
</pre>

<pre>
<code>
use_functions = [
    {
        "type": "function",
        "function": {
        "name": "predict_time_to_reach_threshold",
        "description": "지정한 임계값(ppm)에 도달하는데 걸리는 예상 시간을 반환",
        "parameters": {
            "type": "object",
            "properties": {
                "threshold": {
                    "type": "number",
                    "description": "CO 임계값(ppm)"
                }
            },
            "required": ["threshold"]
            }
        }
    },
    {
        "type": "function",
        "function": {
        "name": "add_ppm_data",
        "description": "1분간 CO 데이터를 모아주는 함",
        "parameters": {
            "type": "object",
            "properties": {
                "ppm": {
                    "type": "number",
                    "description": "CO 측정"
                }
            },
            "required": ["ppm"]
            }
        }
    }
]
</code>
</pre>

<pre>
<code>
def ask_openai(llm_model, messages, user_message, functions = ''):
    client = OpenAI()
    proc_messages = messages

    if user_message != '':
        proc_messages.append({"role": "user", "content": user_message})

    if functions == '':
        response = client.chat.completions.create(model=llm_model, messages=proc_messages, temperature = 1.0)
    else:
        response = client.chat.completions.create(model=llm_model, messages=proc_messages, tools=use_functions, tool_choice="auto") # 이전 코드와 바뀐 부분

    response_message = response.choices[0].message
    tool_calls = response_message.tool_calls

    if tool_calls:
        # Step 3: call the function
        # Note: the JSON response may not always be valid; be sure to handle errors

        available_functions = {
            # "add_ppm_data": add_ppm_data,		
            "predict_time_to_reach_threshold": predict_time_to_reach_threshold
        }

        messages.append(response_message)  # extend conversation with assistant's reply
        print(response_message)
        # Step 4: send the info for each function call and function response to GPT
        for tool_call in tool_calls:
            function_name = tool_call.function.name
            function_to_call = available_functions[function_name]
            function_args = json.loads(tool_call.function.arguments)
            print(function_args)

            if 'user_prompt' in function_args:
                function_response = function_to_call(function_args.get('user_prompt'))
            else:
                function_response = function_to_call(**function_args)

            proc_messages.append(
                {
                    "tool_call_id": tool_call.id,
                    "role": "tool",
                    "name": function_name,
                    "content": function_response,
                }
            )  # extend conversation with function response
        second_response = client.chat.completions.create(
            model=llm_model,
            messages=messages,
        )  # get a new response from GPT where it can see the function response

        assistant_message = second_response.choices[0].message.content
    else:
        assistant_message = response_message.content

    text = assistant_message.replace('\n', ' ').replace(' .', '.').strip()


    proc_messages.append({"role": "assistant", "content": assistant_message})

    return text # proc_messages, 
</code>
</pre>

<pre>
<code>
import gradio as gr

def gradio_interface(user_message):
    messages = [
    {"role": "system", "content": "당신은 CO 농도 예측 전문가입니다. 사용자 질문에 대해 필요하면 함수를 호출하여 답해주세요."},
    {"role": "user", "content": "임계값은 10입니다. 환기가 언제 필요할까요?"}
    ]
    answer = ask_openai("gpt-4o-mini", messages, user_message, functions= use_functions)
    return answer



# Gradio 인터페이스
iface = gr.Interface(fn=gradio_interface,
                     inputs="text",
                     outputs="text",
                     title="CO 모니터링 & 예측 챗봇",
                     description="CO 농도 상태 및 환기가 필요한 시간 예측")
</code>
</pre>

<pre>
<code>
from datetime import timedelta
init_csv_file(CSV_FILE_PATH)

try:
    ser = serial.Serial(PORT, 9600, timeout=1)
    print(f"Arduino 연결됨: {PORT}")
    time.sleep(1)  # 시리얼 초기화 대기 시간

    # Gradio 인터페이스 별도 스레드에서 실행
    import threading
    server_thread = threading.Thread(target=iface.launch, kwargs={"debug":True}, daemon=True)
    server_thread.start()

    while True:
        if ser.in_waiting > 0:
            line = ser.readline().decode('utf-8').strip()
            try:
                ppm = float(line)
                current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                append_csv_file(CSV_FILE_PATH, current_time, ppm)
                print(f"CO 농도: {ppm:.2f} ppm")

                add_ppm_data(ppm)

                status, _ = get_co_status(ppm)
                if status in ["주의", "위험", "매우 위험"]:
                    send_discord_alert(ppm)

            except ValueError:
                print(f"잘못된 데이터 수신: {line}")

        # 측정 주기 1초
        time.sleep(1)

except serial.SerialException as e:
    print(f"시리얼 연결 오류: {str(e)}")
except KeyboardInterrupt:
    print("프로그램 종료 요청 받음")
finally:
    if 'ser' in locals() and ser.is_open:
        ser.close()
</code>
</pre>
