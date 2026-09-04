# ======================================================
# 🤖 CHATBOT — CUIDADOS DE MASCOTAS
# Interfaz gráfica con Tkinter
# ======================================================

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
        
        (r"alimentar|comida|alimentación|qué come|alimentar",
         "🥩 La alimentación depende del animal:\n"
         "🐶 Perro: 2 comidas al día, alimento balanceado de buena calidad.\n"
         "🐱 Gato: Alimento específico, rico en proteínas.\n"
         "🐰 Conejo: Principalmente heno y verduras frescas."),
        
        (r"vacuna|vacunas|cuándo vacunar|vacunación",
         "💉 Las vacunas son muy importantes:\n"
         "- Perros y gatos: vacuna contra rabia + vacuna polivalente desde las 8 semanas.\n"
         "- Refuerzo: una vez al año. Consulta siempre con tu veterinario."),
        
        (r"baño|bañar|cada cuánto baño|higiene",
         "🛁 Frecuencia recomendada:\n"
         "- Perros: cada 1 o 2 meses.\n"
         "- Gatos: se limpian solos, baño solo si es necesario.\n"
         "⚠️ Usa siempre productos para animales, NUNCA jabón de humanos."),
        
        (r"ejercicio|pasear|cuánto ejercicio|actividad",
         "🏃 El ejercicio varía por raza y edad:\n"
         "- Perros adultos: 2 paseos diarios de 30 minutos mínimo.\n"
         "- Razas pequeñas: menor exigencia física.\n"
         "- Gatos: juego en casa con juguetes."),
        
        (r"enfermo|enferma|siento mal|vómito|diarrea|llevar veterinario",
         "⚠️ Si notas: falta de apetito, vómitos, diarrea, decaimiento o heridas →\n"
         "Llévalo al veterinario cuanto antes. ¡No le des medicamentos sin consultar!"),
        
        (r"gracias|adiós|chao|hasta luego|nos vemos",
         "¡Con gusto! 🐾 Cuida mucho a tu mascota. ¡Hasta pronto!"),
    ]

    # Buscar coincidencias
    for patron, respuesta in respuestas:
        if re.search(patron, mensaje):
            return respuesta

    # Respuesta por defecto
    return "Lo siento, no entiendo tu pregunta. Puedes preguntarme sobre: 🥩comida, 💉vacunas, 🛁baños, 🏃ejercicio o ⚠️salud de tu mascota."


# ---------------------- INTERFAZ GRÁFICA ----------------------
def enviar_mensaje():
    texto_usuario = entrada.get().strip()
    if not texto_usuario:
        return

    # Mostrar mensaje del usuario
    chat.config(state=tk.NORMAL)
    chat.insert(tk.END, f"Tú: {texto_usuario}\n", "usuario")
    chat.yview(tk.END)

    # Obtener y mostrar respuesta del bot
    respuesta = obtener_respuesta(texto_usuario)
    chat.insert(tk.END, f"Bot: {respuesta}\n\n", "bot")
    chat.yview(tk.END)
    chat.config(state=tk.DISABLED)

    # Limpiar campo de entrada
    entrada.delete(0, tk.END)


# Crear ventana principal
ventana = tk.Tk()
ventana.title("🐾 Chatbot — Cuidados de Mascotas")
ventana.geometry("550x600")
ventana.resizable(True, True)

# Área del historial del chat
chat = scrolledtext.ScrolledText(ventana, wrap=tk.WORD, font=("Arial", 11))
chat.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)
chat.config(state=tk.DISABLED)  # Solo lectura

# Colores para diferenciar mensajes
chat.tag_config("usuario", foreground="#2c3e50", font=("Arial", 11, "bold"))
chat.tag_config("bot", foreground="#27ae60", font=("Arial", 11))

# Mensaje de bienvenida
chat.config(state=tk.NORMAL)
chat.insert(tk.END, "Bot: ¡Hola! 🐾 Soy tu asistente sobre cuidados de mascotas.\n"
                     "Pregúntame sobre comida, vacunas, baños, ejercicio o salud.\n\n", "bot")
chat.config(state=tk.DISABLED)

# Campo de entrada y botón
marco = tk.Frame(ventana)
marco.pack(padx=10, pady=(0, 10), fill=tk.X)

entrada = tk.Entry(marco, font=("Arial", 11))
entrada.pack(side=tk.LEFT, fill=tk.X, expand=True, padx=(0, 8))
entrada.focus()

boton = tk.Button(marco, text="Enviar", command=enviar_mensaje,
                  bg="#3498db", fg="white", font=("Arial", 10, "bold"))
boton.pack(side=tk.RIGHT)

# Permitir enviar presionando la tecla Enter
ventana.bind("<Return>", lambda e: enviar_mensaje())

# Ejecutar la aplicación
ventana.mainloop()

