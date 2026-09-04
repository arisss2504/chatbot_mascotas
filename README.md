# Chatbot sobre Cuidados de Mascotas
# Interfaz gráfica con Tkinter

import tkinter as tk
from tkinter import scrolledtext
import re

# ---------------------- LÓGICA DEL CHATBOT ----------------------
def obtener_respuesta(mensaje):
    """Devuelve una respuesta según las palabras clave del usuario."""
    mensaje = mensaje.lower()

    # Patrones de palabras clave y respuestas
    respuestas = [
        (r"hola|buenos días|buenas tardes|buenas noches",
         "¡Hola! 🐶 Bienvenido/a al asistente de cuidados de mascotas. ¿En qué te puedo ayudar hoy?"),
        
        (r"alimentar|comida|alimentación|qué come",
         "La alimentación depende del animal:\n🐶 Perro: 2 comidas al día, balanceado de buena calidad.\n🐱 Gato: Alimento específico, rico en proteínas.\n🐰 Conejo: Principalmente heno y verduras frescas."),
        
        (r"vacuna|vacunas|cuándo vacunar",
         "💉 Las vacunas son muy importantes:\n- Perros y gatos: vacuna contra rabia + vacuna polivalente desde las 8 semanas.\n- Refuerzo anual: una vez al año. Consulta siempre con tu veterinario."),
        
        (r"baño|bañar|cada cuánto baño",
         "🛁 Frecuencia recomendada:\n- Perros: cada 1 o 2 meses.\n- Gatos: se limpian solos, baño solo si es necesario.\n- Usa siempre productos especiales para animales, NUNCA jabón de humanos."),
        
        (r"ejercicio|pasear|cuánto ejercicio",
         "🏃‍♂️ El ejercicio varía por raza y edad:\n- Perros adultos: 2 paseos diarios de 30 min mínimo.\n- Razas pequeñas: menor exigencia.\n- Gatos: juego en casa con juguetes."),
        
        (r"enfermo|enferma|siento mal|llevar veterinario",
         "⚠️ Si notas: falta de apetito, vómitos, diarrea, decaimiento o heridas → Llévalo al veterinario cuanto antes. No te automediques sin consultar."),
        
        (r"adiós|chao|hasta luego|gracias",
         "¡Con gusto! 🐾 Cuida mucho a tu mascota. ¡Hasta pronto!"),
    ]

    # Buscar coincidencias
    for patron, respuesta in respuestas:
        if re.search(patron, mensaje):
            return respuesta

    # Respuesta por defecto
    return "Lo siento, no entiendo tu consulta. Pregúntame sobre: comida, vacunas, baños, ejercicio o salud de tu mascota 🐾"

# ---------------------- INTERFAZ GRÁFICA ----------------------
def enviar_mensaje():
    texto_usuario = entrada.get().strip()
    if not texto_usuario:
        return

    # Mostrar mensaje del usuario
    chat.config(state=tk.NORMAL)
    chat.insert(tk.END, f"Tú: {texto_usuario}\n", "usuario")
    chat.yview(tk.END)

    # Obtener y mostrar respuesta
    respuesta = obtener_respuesta(texto_usuario)
    chat.insert(tk.END, f"Bot: {respuesta}\n\n", "bot")
    chat.yview(tk.END)
    chat.config(state=tk.DISABLED)

    # Limpiar entrada
    entrada.delete(0, tk.END)


# Crear ventana principal
ventana = tk.Tk()
ventana.title("🐾 Chatbot — Cuidados de Mascotas")
ventana.geometry("550x600")
ventana.resizable(True, True)

# Área del chat
chat = scrolledtext.ScrolledText(ventana, wrap=tk.WORD, font=("Arial", 11))
chat.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)
chat.config(state=tk.DISABLED)  # Solo lectura

# Colores para mensajes
chat.tag_config("usuario", foreground="#2c3e50", font=("Arial", 11, "bold"))
chat.tag_config("bot", foreground="#27ae60", font=("Arial", 11))

# Mensaje de bienvenida
chat.config(state=tk.NORMAL)
chat.insert(tk.END, "Bot: ¡Hola! 🐾 Soy tu asistente sobre cuidados de mascotas.\nPregúntame sobre comida, vacunas, baños, ejercicio o salud.\n\n", "bot")
chat.config(state=tk.DISABLED)

# Marco de entrada
marco = tk.Frame(ventana)
marco.pack(padx=10, pady=(0, 10), fill=tk.X)

entrada = tk.Entry(marco, font=("Arial", 11))
entrada.pack(side=tk.LEFT, fill=tk.X, expand=True, padx=(0, 8))
entrada.focus()

boton = tk.Button(marco, text="Enviar", command=enviar_mensaje,
                  bg="#3498db", fg="white", font=("Arial", 10, "bold"))
boton.pack(side=tk.RIGHT)

# Permitir enviar con Enter
ventana.bind("<Return>", lambda e: enviar_mensaje())

# Ejecutar app
ventana.mainloop()
