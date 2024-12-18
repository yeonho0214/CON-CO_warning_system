CON-CO_warning_system
=====================
## Background: Carbon Monoxide (CO)
### 1. What is Carbon Monoxide?
* A colorless, odorless gas produced when fuels like coal or oil burn in low oxygen conditions.
* Main sources: charcoal briquettes, combustion gases, vehicle exhaust, wildfires.

### 2. Carbon Monoxide Poisoning
* Main Symptoms: Headache, nausea, vomiting, tinnitus, difficulty breathing, increased heart rate.
* Effects on Humans by CO Concentration:
> * 3200 ppm - Headache, dizziness, vomiting within 5 minutes; death within 20 minutes
> * 1600 ppm - Headache, dizziness, vomiting within 10 minutes; death within 30 minutes
> * 800 ppm - Headache, dizziness, vomiting within 20 minutes; death within 1 hour
> * 400 ppm - Headache, dizziness, vomiting within 1 hour; death within 3 hours
> * 200 ppm - Headache, fatigue, dizziness within 2-3 hours
> * 50 ppm - Minimal impact, acceptable workplace exposure limit.

### 3. CO Poisoning Accidents During Camping
> <img src="https://github.com/user-attachments/assets/b12e9047-c89a-4298-837d-5acabe06681d" width="80%" />

> * Trend: CO poisoning incidents are increasing each year

## Sensor and Buzzer Assembly with Arduino

### 1. MQ-9 Gas Sensor 
#### <Introduction>
* The MQ-9 gas sensor detects the concentration of various gases, particularly carbon monoxide (CO), methane (CH₄), and liquefied petroleum gas (LPG). It is used for gas leak detection, air quality monitoring, and industrial safety systems.
#### <December 2nd (Mon) Attempt: Connection and Measurement with JETBOT>
> <img src="https://github.com/user-attachments/assets/f15ed0e5-3371-40a2-96b0-ed486a6294dc" alt="Image 1" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/12600b08-ccf8-4090-a4b6-e7ea8cf2b8ab" alt="Image 2" style="width: 30%;"/>

> <img src="https://github.com/user-attachments/assets/d4c0ebcd-934b-4385-bb7f-859cee0ae6cc" alt="Image 2" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/087457fa-1963-446a-8c8b-a9a2f2d3a42c" alt="Image" style="width: 30%;"/>

> * Issue: Despite following blog instructions, the concentration did not vary significantly. (Link to the blog : [https://cafe.naver.com/mechawiki/3521])
> * Solution: Replaced MQ-9 with MQ-7 as MQ-7 is specialized for CO detection
>    * MQ-7 Sensor: Focuses solely on CO detection with a wider range (20-2000 ppm) and stable operation at higher concentrations.
>    * MQ-9 Sensor: Less sensitive to specific gases like CO due to its broad detection range.

* Code for MQ-9 Usage :
<pre>
<code>
const int MQ9_AOUT_PIN = A0; // Analog pin connected to the AOUT pin of the MQ-9 sensor
float sensorValue;           // Sensor output value

void setup() {
  Serial.begin(9600); // Start serial communication
  pinMode(MQ9_AOUT_PIN, INPUT); // Set MQ-9 pin as input
}

void loop() {
  // Read analog value
  sensorValue = analogRead(MQ9_AOUT_PIN);

  // Convert sensor value (0~1023 → gas concentration)
  float voltage = sensorValue * (5.0 / 1023.0); // Convert to voltage assuming 5V reference
  float co_concentration = voltage * 100;      // Arbitrary concentration calculation (calibration required)

  // Output to serial monitor
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(co_concentration);
  Serial.println(" ppm"); // Output in ppm units (calibrated concentration)

  delay(1000); // Update every 1 second
}
</code>
</pre>


***
### 2. MQ-7 Gas Senso
#### <Introduction>
> <img src="https://github.com/user-attachments/assets/bfb6f645-0cb4-4575-8f2f-348e193f5dd4" width="40%" />
* MQ-7 is specialized for carbon monoxide (CO) detection with high sensitivity and a measurement range of 20-2000 ppm. It operates by periodically changing heater voltage, ideal for boiler rooms and vehicle exhaust detection.
* Operating Mechanism:
  * <img src="https://github.com/user-attachments/assets/fb709418-5ef7-4d99-9015-0b68cc8758cf" width="40%" />

#### <December 4th (Wed) Attempt: Connection and Measurement with JETBOT>
> <img src="https://github.com/user-attachments/assets/d91a8dbe-538e-4ac1-be59-bfde55e4357c" alt="Image 1" style="width: 30%;"/>
> <img src="https://github.com/user-attachments/assets/c4af8b61-ead2-4e2d-98b6-d1453c07f33d" alt="Image 2" style="width: 50%;"/>

> * Plan: Use Discord for threshold alerts and add a buzzer for audible warnings.
> * Calibration: Chose constants from the datasheet (a=100, b=-1.5) for ppm conversion

***
### 3. Discord Alerts and Buzzer Notifications
#### <Introduction>
* Send alerts via Discord when CO concentration exceeds thresholds.
    * <img src="https://github.com/user-attachments/assets/123dbc8f-32cc-46bb-8721-32488e455aec" width="50%" />

* Integrate an active piezo buzzer for audible alarms at specific CO levels
    * Different frequencies, durations, and pauses correspond to CO levels.
    * <img src="https://github.com/user-attachments/assets/27d8d720-a6f5-44b3-8561-f2664e0d9454" width="50%" />



#### <December 7th (Sat) Experiment>
> <img src="https://github.com/user-attachments/assets/cb513ab1-e134-4dc4-8d2e-038b23f017f9" alt="Image 1" style="width: 40%;"/>
> <img src="https://github.com/user-attachments/assets/b747a1c3-58d0-4578-9070-5692ccf27e07" alt="Image 3" style="width: 50%;"/>
> <img src="https://github.com/user-attachments/assets/b470708c-b824-4159-8042-7b94a2f51f14" alt="Image 4" style="width: 40%;"/>
> <img src="https://github.com/user-attachments/assets/fbc4c6bf-ba47-4d46-963b-7fb9b8d64b3e" alt="Image 5" style="width: 40%;"/>

> * Used 1.5g charcoal briquettes (3 pieces) for the final experiment.
> * Initial thresholds were set at 200, 800, and 3200 ppm based on WHO recommendations.
> * Issue: CO concentration did not rise significantly due to environmental limitations
>    * Attempts: Used 1-5 candles and small charcoal pieces, but levels remained low.
>    * Solution: Adjusted threshold settings to lower levels.
>        * <img src="https://github.com/user-attachments/assets/ab9dee1c-4524-4301-b318-69248710e9f6" width="50%" />


* Code for MQ-7 Gas Sensor, Discord Notification Transmission, and Buzzer Alert 
<pre>
<code>
const int MQ7_AOUT_PIN = A0; // Analog pin connected to the AOUT pin of the MQ-7 sensor
const float VCC = 5.0;       // Arduino supply voltage
const float RL = 10.0;       // Load resistance (unit: kΩ)
const float R0 = 10.0;       // Sensor resistance in clean air (unit: kΩ)
const float a = 100.0;       // Constant 'a' provided in the datasheet
const float b = -1.5;        // Constant 'b' provided in the datasheet

void setup() {
  Serial.begin(9600); // Start serial communication
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // Read analog value
  float voltage = (sensorValue / 1023.0) * VCC; // Calculate voltage
  float RS = RL * (VCC - voltage) / voltage;   // Calculate sensor resistance
  float ratio = RS / R0;                       // Calculate sensor ratio
  float ppm = a * pow(ratio, b);               // Calculate PPM

  // Output results
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(ppm);
  Serial.println(" ppm");

  delay(1000); // Update every 1 second
}
</code>
</pre>


<pre>
<code>
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "Your_SSID";      // Wi-Fi name
const char* password = "Your_PASS";  // Wi-Fi password
const char* webhook_url = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"; // Discord webhook URL

const int MQ7_AOUT_PIN = A0; // MQ-7 AOUT pin
const float VCC = 5.0;       // Arduino supply voltage
const float RL = 10.0;       // Load resistance (kΩ)
const float R0 = 10.0;       // Sensor resistance in clean air (kΩ)
const float a = 100.0;       // Constant 'a' from the datasheet
const float b = -1.5;        // Constant 'b' from the datasheet
float ppm_threshold = 200.0; // Warning threshold (unit: ppm)

void setup() {
  Serial.begin(9600);
  pinMode(MQ7_AOUT_PIN, INPUT);

  // Wi-Fi connection
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nConnected to WiFi!");
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // Read analog value
  float voltage = (sensorValue / 1023.0) * VCC; // Calculate voltage
  float RS = RL * (VCC - voltage) / voltage;   // Calculate sensor resistance
  float ratio = RS / R0;                       // Calculate sensor ratio
  float ppm = a * pow(ratio, b);               // Calculate PPM

  // Output results
  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | Voltage: ");
  Serial.print(voltage);
  Serial.print(" V | CO Concentration: ");
  Serial.print(ppm);
  Serial.println(" ppm");

  // Send a warning to Discord if concentration exceeds threshold
  if (ppm > ppm_threshold) {
    sendToDiscord(ppm);
  }

  delay(1000); // Update every 1 second
}

// Function to send a message to Discord
void sendToDiscord(float ppm) {
  if (WiFi.status() == WL_CONNECTED) { // Check Wi-Fi connection
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

    http.end(); // End HTTP connection
  } else {
    Serial.println("WiFi not connected!");
  }
}

</code>
</pre>



<pre>
<code>
const int MQ7_AOUT_PIN = A0; // Analog output pin of the MQ-7 sensor

const float VCC = 5.0;       // Arduino supply voltage
const float RL = 10.0;       // Load resistance (unit: kΩ)
const float R0 = 10.0;       // Sensor resistance in clean air (unit: kΩ)
const float a = 100.0;       // Constant 'a' provided in the datasheet
const float b = -1.5;        // Constant 'b' provided in the datasheet

void setup() {
  Serial.begin(9600); // Start serial communication
}

void loop() {
  int sensorValue = analogRead(MQ7_AOUT_PIN); // Read sensor value
  float voltage = (sensorValue / 1023.0) * VCC; // Calculate voltage
  float RS = RL * (VCC - voltage) / voltage;   // Calculate sensor resistance
  float ratio = RS / R0;                       // Calculate sensor ratio
  float ppm = a * pow(ratio, b);               // Calculate PPM

  // Send data to the serial port
  Serial.println(ppm);
  delay(1000); // 1-second interval
}
</code>
</pre>

* Python part
  
<pre>
<code>
import serial
import time
import requests
from datetime import datetime

# Set the serial port for connection to Arduino
PORT = "COM3"  # Port where Arduino is connected (Windows: COMx, Mac/Linux: /dev/ttyUSBx)
BAUDRATE = 9600  # Baud rate matching Arduino's setting
WEBHOOK_URL = "YOUR_DISCORD_WEBHOOK_URL"  # Enter Discord webhook URL

# CO warning threshold
CO_THRESHOLD = 50.0  # In PPM units

def send_discord_alert(ppm):
    """Send a warning message to Discord"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    color = 0xFF0000 if ppm >= CO_THRESHOLD else 0xFFFF00

    data = {
        "embeds": [{
            "title": "⚠️ CO Concentration Warning ⚠️",
            "description": f"Current CO concentration is {ppm:.2f} ppm.",
            "color": color,
            "fields": [
                {"name": "Status", "value": "Danger Level Reached" if ppm >= CO_THRESHOLD else "Normal", "inline": True},
                {"name": "Measurement Time", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO Monitoring System"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] Discord notification sent successfully: {ppm:.2f} ppm")
        else:
            print(f"Discord notification failed: {response.status_code}")
    except Exception as e:
        print(f"Discord notification error: {str(e)}")

def main():
    try:
        # Open Arduino serial port
        ser = serial.Serial(PORT, BAUDRATE, timeout=1)
        print(f"Connected to Arduino: {PORT}")
        time.sleep(2)  # Wait for Arduino initialization

        while True:
            # Read serial data
            if ser.in_waiting > 0:
                line = ser.readline().decode('utf-8').strip()
                try:
                    ppm = float(line)
                    print(f"CO Concentration: {ppm:.2f} ppm")

                    # Send a Discord alert if CO concentration exceeds the threshold
                    if ppm >= CO_THRESHOLD:
                        send_discord_alert(ppm)

                except ValueError:
                    print(f"Invalid data received: {line}")
            time.sleep(1)

    except serial.SerialException as e:
        print(f"Serial connection error: {str(e)}")
    except KeyboardInterrupt:
        print("Program terminated")
    finally:
        if 'ser' in locals() and ser.is_open:
            ser.close()
            print("Serial port closed")

if __name__ == "__main__":
    main()
</code>
</pre>

## Ventilation Prediction System
#### <Introsuction>
* Purpose: Predict appropriate ventilation times based on CO concentration trends.
* Technology Used:
    * Linear Regression: Predict threshold arrival time based on concentration trends.
    * Gradio-Based Chatbot: Users ask questions, and the system responds with ventilation advice.
* Principle (Visualization):
> <img src="https://github.com/user-attachments/assets/4ac5c5f7-ab74-40dc-a204-7cbe63d1e8a7" width="80%" /> 
#### <CO Monitoring & Prediction Chatbot Examples>
> (Example 1)
> <img src="https://github.com/user-attachments/assets/748b8012-20eb-4c25-83a5-3323375beb29" width="80%" />

> (Example 2)
> <img src="https://github.com/user-attachments/assets/96b9d44c-d844-480e-9bc4-e95d7c51c45b" width="80%" />

* Simple Explanation of the Ventilation Prediction System Code
> 1.  Handling User Queries via OpenAI Function : When a user inputs a query related to CO concentration, the system provides an appropriate response or predicts the time required to reach the CO threshold.
>    * 'openai_chat(query)' : Calls the function to calculate the time to reach the threshold if necessary, and generates a response.
>    * 'gradio_interface(query)' : Connects the OpenAI Function's output to the Gradio interface for display.

> 2. Gradio Interface : Provides a user-friendly web interface to handle queries about CO status and predictions.
>    * Input: Text-based queries (e.g., "How long will it take to reach 300ppm?").
>    * Output: Predicted time or status information.
>    * Web Server: Runs through the web UI provided by Gradio.

> 3. CO Concentration Data Processing and Alerts
>   * Data Collection
>     * Receives CO concentration data from Arduino via serial communication.
>     * Reads data at 1-second intervals, saves it in a CSV file, and appends it to a buffer (ppm_buffer).
>     * Exception Handling: Ignores invalid data formats and logs them.
>   * Status Monitoring and Alerts
>     * 'get_co_status(ppm)' : Determines the CO concentration status (Normal, Caution, Danger, Severe Danger).
>     * 'send_discord_alert(ppm)' : Sends alerts to Discord for statuses above "Caution".

> 4. CSV File Initialization and Data Storage
>   * 'init_csv_file' : Creates the directory and CSV file at the specified path, adding headers.
>   * 'append_csv_file' : Appends timestamp and ppm values to the CSV file.

> 5. CO Concentration Prediction
>   * 'add_ppm_data(ppm)' : Manages data for the last minute by removing outdated entries.
>   * 'predict_time_to_reach_threshold(threshold)':
>     * Analyzes recent data using linear regression to predict the time required to reach a specific ppm threshold.
>     * Returns appropriate messages if the concentration is not increasing or if there is insufficient data.

> 6. Multi-Threaded Server Execution : Utilizes Python threads to run the Gradio server and real-time data collection loop simultaneously.
>    * Gradio Interface Execution: Runs 'iface.launch' in a separate thread.
>    * CO Concentration Data Collection: Continuously reads and processes serial data in the main thread.

> 7. Exception and Termination Handling
>    * Serial Connection Failure : Prints an error message if Arduino fails to connect.
>    * Keyboard Interrupt : Safely terminates the program upon user request.
>    * Resource Cleanup : Closes the serial port to release resources.

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
import serial
import time
import requests
from datetime import datetime, timedelta
import csv
import os
import numpy as np
from openai import OpenAI
import gradio as gr
import json

# ===== Parts that need to be modified for your environment =====
PORT = "/dev/ttyUSB0"  # Check the Arduino port on Jetson Nano (e.g., /dev/ttyACM0)
WEBHOOK_URL = "https://discord.com/api/webhooks/1313826821787226132/txS4YAXl6tm_5UWQVzSCX0rQRLGOOELs2a_9PIk3vMNALzxxX2r88bDJcZ6f0K5v_3oe"  # Actual Discord Webhook URL
CSV_FILE_PATH = "/home/dli/CO_ver2/co_readings_gradio.csv"  # Path to save the CSV file
# =============================================================

# Status criteria
# Normal: ppm < 200
# Caution: 200 ≤ ppm < 800
# Danger: 800 ≤ ppm < 3200
# Critical: ppm ≥ 3200

def init_csv_file(filename):
    dir_path = os.path.dirname(filename)
    if dir_path and not os.path.exists(dir_path):
        os.makedirs(dir_path, exist_ok=True)
    # Write headers if the file does not exist
    if not os.path.exists(filename):
        with open(filename, mode='w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(["timestamp", "ppm"])
        print(f"CSV file created and header written: {filename}")

def append_csv_file(filename, timestamp_str, ppm_value):
    with open(filename, mode='a', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow([timestamp_str, ppm_value])

def get_co_status(ppm):
    if ppm < 0.6:
        return "Normal", 0x00FF00  # Green
    elif ppm < 5:
        return "Caution", 0xFFFF00  # Yellow
    elif ppm < 20:
        return "Danger", 0xFFA500  # Orange
    else:
        return "Critical", 0xFF0000  # Red
</code>
</pre>

<pre>
<code>
def send_discord_alert(ppm):
    """Send alerts based on CO concentration levels"""
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    status, color = get_co_status(ppm)
    description = f"The current CO concentration is {ppm:.2f} ppm."
    title = f"⚠️ CO {status} Status ⚠️"

    data = {
        "content": "@here",
        "embeds": [{
            "title": title,
            "description": description,
            "color": color,
            "fields": [
                {"name": "Status", "value": status, "inline": True},
                {"name": "Measurement Time", "value": current_time, "inline": False}
            ],
            "footer": {"text": "CO Monitoring System"}
        }]
    }

    try:
        response = requests.post(WEBHOOK_URL, json=data)
        if response.status_code == 204:
            print(f"[{current_time}] Discord alert sent successfully: {ppm:.2f} ppm ({status})")
        else:
            print(f"Failed to send Discord alert: {response.status_code}")
    except Exception as e:
        print(f"Discord alert error: {str(e)}")
</code>
</pre>

<pre>
<code>
# Store data for the last 1 minute (1-second intervals -> 60 data points)
ppm_buffer = []

def add_ppm_data(ppm):
    ppm_buffer.append((datetime.now(), ppm))
    # Remove data older than 1 minute
    cutoff = datetime.now() - timedelta(seconds=60)
    while ppm_buffer and ppm_buffer[0][0] < cutoff:
        ppm_buffer.pop(0)

def predict_time_to_reach_threshold(threshold):
    # Use data from the last 1 minute
    # ppm_buffer: [(time, ppm), ...]
    if len(ppm_buffer) < 2:
        return "Not enough data for prediction."

    # Convert the time axis to seconds
    base_time = ppm_buffer[0][0]
    times = np.array([(t[0]-base_time).total_seconds() for t in ppm_buffer])
    ppms = np.array([t[1] for t in ppm_buffer])

    # Linear regression
    # y = m*x + c
    # np.polyfit(x, y, 1) -> (m, c)
    m, c = np.polyfit(times, ppms, 1)

    # Assume ppm continues to increase with slope m (gradient)
    # threshold = m*x + c -> x = (threshold - c)/m
    if m <= 0:
        return "CO concentration is not increasing. Ventilation may not be urgent."

    x_threshold = (threshold - c)/m
    current_time_sec = (ppm_buffer[-1][0]-base_time).total_seconds()

    if x_threshold <= current_time_sec:
        return "The threshold already seems to have been reached."

    delta = x_threshold - current_time_sec
    hours = delta // 3600
    minutes = (delta % 3600) // 60
    seconds = delta % 60
    return f"In approximately {int(hours)} hours {int(minutes)} minutes {int(seconds)} seconds, CO concentration is expected to reach {threshold} ppm."

</code>
</pre>

<pre>
<code>
use_functions = [
    {
        "type": "function",
        "function": {
        "name": "predict_time_to_reach_threshold",
        "description": "Returns the estimated time to reach the specified threshold (ppm)",
        "parameters": {
            "type": "object",
            "properties": {
                "threshold": {
                    "type": "number",
                    "description": "CO threshold (ppm)"
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
        "description": "Collects CO data for 1 minute",
        "parameters": {
            "type": "object",
            "properties": {
                "ppm": {
                    "type": "number",
                    "description": "CO measurement"
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
        response = client.chat.completions.create(model=llm_model, messages=proc_messages, tools=use_functions, tool_choice="auto") # Changes from the previous code

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
    {"role": "system", "content": "You are an expert in CO concentration prediction. If necessary, call functions to answer user questions."},
    {"role": "user", "content": "The threshold is 10. When will ventilation be needed?"}
    ]
    answer = ask_openai("gpt-4o-mini", messages, user_message, functions=use_functions)
    return answer

# Gradio interface
iface = gr.Interface(fn=gradio_interface,
                     inputs="text",
                     outputs="text",
                     title="CO Monitoring & Prediction Chatbot",
                     description="Predicts CO concentration status and when ventilation will be needed.")
</code>
</pre>

<pre>
<code>
from datetime import timedelta
init_csv_file(CSV_FILE_PATH)

try:
    ser = serial.Serial(PORT, 9600, timeout=1)
    print(f"Arduino connected: {PORT}")
    time.sleep(1)  # Wait time for serial initialization

    # Run Gradio interface in a separate thread
    import threading
    server_thread = threading.Thread(target=iface.launch, kwargs={"debug": True}, daemon=True)
    server_thread.start()

    while True:
        if ser.in_waiting > 0:
            line = ser.readline().decode('utf-8').strip()
            try:
                ppm = float(line)
                current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                append_csv_file(CSV_FILE_PATH, current_time, ppm)
                print(f"CO concentration: {ppm:.2f} ppm")

                add_ppm_data(ppm)

                status, _ = get_co_status(ppm)
                if status in ["Caution", "Danger", "Critical"]:
                    send_discord_alert(ppm)

            except ValueError:
                print(f"Invalid data received: {line}")

        # Measurement interval: 1 second
        time.sleep(1)

except serial.SerialException as e:
    print(f"Serial connection error: {str(e)}")
except KeyboardInterrupt:
    print("Program termination request received")
finally:
    if 'ser' in locals() and ser.is_open:
        ser.close()
</code>
</pre>
***
## Conclusion

#### <Points for Improvement>
> * Limitations in Experimental Environment and Threshold Settings
>     * Could not utilize actual CO (ppm) reference data, and proceeded based on arbitrary values due to experimental constraints.
> * Simplicity of Analysis
>     * Applied only linear regression to the ventilation prediction system, without comparing or utilizing various algorithms.
> * Limited Scope of Data Usage
>     * Analysis was based on data from only the past minute, without reflecting a larger dataset.

#### <Suggestions for Improvement>
> * Collect more data and apply various algorithms to develop a highly reliable and practical system.
>    * 1) CO Concentration Data: Enhance model reliability by collecting CO concentration data over extended periods in diverse environments.
>    * 2) Various Algorithms: Compare and analyze different algorithms to select the optimal methodology.
>    * 3) Practical System: Develop a practical system that can be utilized across diverse environments.
***
# <Code Refinement Starts Here> (Including Trial and Error)

## #1 Arduino IDE
> * Converting the analog values output by the MQ-7 sensor into ppm units.
> * Ensuring that the buzzer sounds when the defined thresholds (e.g., Normal, Danger, etc.) are exceeded.

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
