# Practica
    from machine import Pin, I2C, Timer
    from ssd1306 import SSD1306_I2C
    import framebuf
    import urequests
    import ujson

# Configura el pin del botón
    button = Pin(2, Pin.IN, Pin.PULL_UP)

# Configura la pantalla OLED
    i2c = I2C(0, sda=Pin(0), scl=Pin(1), freq=400000)
    oled = SSD1306_I2C(128, 64, i2c)

# Clave de API de OpenAI (reemplaza con tu propia clave)
    api_key = "YOUR_API_KEY"

# URL de la API de OpenAI para ChatGPT
    url = "https://api.openai.com/v1/engines/gpt-3.5-turbo/completions"

# Configura un temporizador
    timer = Timer()

    def display_response(timer):
# Detiene el temporizador
    timer.deinit()

# Limpia la pantalla
    oled.fill(0)
    oled.text("ChatGPT", 0, 0, 1)  # Agrega "ChatGPT" al inicio
    oled.show()

# Configura la solicitud a la API de OpenAI
    prompt = "Genera una frase aleatoria."
    data = {
        "prompt": prompt,
        "max_tokens": 30  # Cambia el número de tokens según tus necesidades
    }
    headers = {
        "Authorization": "Bearer " + api_key
    }

# Realiza la solicitud a la API de OpenAI
    response = urequests.post(url, json=data, headers=headers)

    if response.status_code == 200:
        response_data = ujson.loads(response.text)
        frase_aleatoria = response_data['choices'][0]['text']
        print("Frase aleatoria generada por ChatGPT:")
        print(frase_aleatoria)

  # Muestra la respuesta en la pantalla
        oled.text(frase_aleatoria, 0, 20, 1)
        oled.show()
    else:
        print("Error al realizar la solicitud a la API de OpenAI")

# Asigna una función al botón
    button.irq(trigger=Pin.IRQ_FALLING, handler=display_response)

# Espera por eventos
    while True:
        pass

