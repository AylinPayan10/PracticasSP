# Desplegar temperatura
![Practica temperatura](https://github.com/AylinPayan10/PracticasSP/assets/143135552/9d25a8b4-dc45-4ba8-ac15-bb0924794c11)
    from machine import Pin, I2C
    from ssd1306 import SSD1306_I2C
    import machine
    import utime

# Inicializa la pantalla OLED
    i2c = I2C(0, sda=Pin(0), scl=Pin(1), freq=400000)
    oled = SSD1306_I2C(128, 64, i2c)

# Configuración del sensor de temperatura
    sensor_temp = machine.ADC(4)
    conversion_factor = 3.3 / (65535)

# Iniciales
    x_initials = 0
    y_initials = 0

    while True:
# Leer la temperatura
    reading = sensor_temp.read_u16() * conversion_factor
    temperature = 27 - (reading - 0.706) / 0.001721
    temperature_str = "{:.1f} C".format(temperature)

# Dibujar la imagen de termómetro en la pantalla
    oled.fill(0)
    oled.text("INICIALES", x_initials, y_initials, 1)  # Iniciales
    oled.text(temperature_str, 0, 40, 1)  # Temperatura

# Mostrar emojis en función del rango de temperatura
    emoji = ":)"  # Carita feliz por defecto
    if temperature <= 22.4:
        # Emoji frío (carita triste)
        emoji = ":("
    elif temperature >= 22.5 and temperature <= 28:
        # Emoji tibio (carita feliz)
        emoji = ":)"
    else:
        # Emoji caliente (carita con lentes)
        emoji = "8-"

    oled.text(emoji, 96, 0, 1)

    oled.show()

# Esperar antes de la siguiente lectura
    utime.sleep(2)

