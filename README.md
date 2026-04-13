# varnikasiri
Myproject1
import speech_recognition as sr
import pyttsx3
import subprocess

# Initialize TTS
engine = pyttsx3.init()

def speak(text):
    print("AI:", text)
    engine.say(text)
    engine.runAndWait()

# Voice recognition
def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)

    try:
        command = recognizer.recognize_google(audio)
        print("You:", command)
        return command.lower()
    except:
        return ""

# Local AI using Ollama (make sure installed)
def ask_ai(prompt):
    try:
        result = subprocess.run(
            ["ollama", "run", "llama2", prompt],
            capture_output=True,
            text=True
        )
        return result.stdout.strip()
    except:
        return "AI service not available"

# Commands
def execute_command(command):
    if "open notepad" in command:
        speak("Opening Notepad")
        subprocess.Popen("notepad.exe")

    elif "time" in command:
        from datetime import datetime
        now = datetime.now().strftime("%H:%M")
        speak(f"The time is {now}")

    elif command:
        response = ask_ai(command)
        speak(response)

# Main loop
if __name__ == "__main__":
    speak("Voice AI Agent Started")
    while True:
        cmd = listen()
        execute_command(cmd)
