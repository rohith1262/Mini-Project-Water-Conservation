# Mini-Project-Water-Conservation
Code For The Smart Water Network Management System
import json
import configuration as conf
import requests
import time

flag = 0

while True:
    url = conf.Thingspeak_url
    msg = requests.request('GET', url)
    response = json.loads(msg.text)
    # print(response)
    data = response['feeds']
    print("this is the difference of flow between sensors : ")
    data1 = data[0]
    data2 = data1['field3']
    print(data2)
    data3 = float(data2)
    if data3 >= 2:
        url1 = conf.Telegram_url
        data = {
            "chat_id": conf.CHAT_ID,
            "text": "Water leakage Detected...."
        }
        try:
            if flag == 0:
                response = requests.request("POST", url1, params=data)
                # print("This is the Telegram URL")
                # print(url)
                print("This is the Telegram response")
                print(response.text)
                flag = 1
        except Exception as e:
            print("An error occurred in sending the alert message via Telegram")
            print(e)
    else:
        flag = 0
        # print("Some Statements")
    time.sleep(10)

