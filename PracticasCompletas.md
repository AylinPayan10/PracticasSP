![Practica1](https://github.com/AylinPayan10/PracticasSP/assets/143135552/9777976b-1adb-4f69-83b4-65fcbab3ac61)
# Codigo 

import requests
from PIL import Image
import time
import pytz
from datetime import datetime
import subprocess

while True:
    response = requests.get("http://worldtimeapi.org/api/timezone/America/Tijuana")
    data = response.json()
    utc_time = datetime.fromisoformat(data["datetime"][:-1]).astimezone(pytz.timezone('America/Los_Angeles'))
    current_time = utc_time.strftime("%H:%M:%S")

    current_hour = int(utc_time.strftime("%H"))
    if 6 <= current_hour < 18:
        image_path = "dia.png"
    elif 18 <= current_hour < 21:
        image_path = "tarde.png"
    else:
        image_path = "noche.png"

    image = Image.open(image_path)
    
    time.sleep(60)

# Librerias 

import machine    
import ssd1306
import network
import urequests
import utime
import ujson


i2c = machine.I2C(0, scl=machine.Pin(1), sda=machine.Pin(0))
oled = ssd1306.SSD1306_I2C(128, 64, i2c)


button = machine.Pin(2, machine.Pin.IN, machine.Pin.PULL_UP)


CHATGPT_API_ENDPOINT = "https://api.openai.com/v1/engines/davinci-codex/completions"
OPENAI_API_KEY = "tu_clave_de_API_de_OpenAI"

def obtener_poema():
    prompt = "Genera un poema sobre la naturaleza"
    headers = {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + OPENAI_API_KEY
    }
    data = {
        "prompt": prompt,
        "max_tokens": 100
    }
    response = urequests.post(CHATGPT_API_ENDPOINT, headers=headers, data=ujson.dumps(data))
    poema = ujson.loads(response.text)["choices"][0]["text"].strip()
    response.close()
    return poema

while True:
   
    if not button.value():
        oled.fill(0)
        oled.text("Generando poema...", 0, 0)
        oled.show()
        
     
        poema = obtener_poema()

       
        oled.fill(0)
        oled.text("Poema generado:", 0, 0)
        oled.text(poema, 0, 20)
        oled.show()

      
        utime.sleep(5)
    


    
