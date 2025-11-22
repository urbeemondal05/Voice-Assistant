# Voice-Assistant


import speech_recognition as sr
import pyttsx3
import datetime
import webbrowser
import os
import time

# Initialize recognizer
r = sr.Recognizer()

def speak(text):
    print("🧠 Misti:", text)
    engine = pyttsx3.init()
    voices = engine.getProperty('voices')
    engine.setProperty('voice', voices[1].id)  # Female voice
    engine.setProperty('rate', 175)
    engine.setProperty('volume', 1.0)
    engine.say(text)
    engine.runAndWait()
    engine.stop()

def listen():
    with sr.Microphone() as source:
        print("🎤 Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        command = r.recognize_google(audio, language="en-in")
        print(f"🗣️ You said: {command}\n")
    except Exception:
        speak("Sorry, I couldn't understand. Please say that again.")
        return ""
    return command.lower()

def main():
    speak("Hi darling! How can I help you?")
    while True:
        command = listen()

        if "time" in command:
            time_now = datetime.datetime.now().strftime("%I:%M %p")
            speak(f"The time is {time_now}")

        elif "date" in command:
            date_now = datetime.datetime.now().strftime("%B %d, %Y")
            speak(f"Today's date is {date_now}")

        elif "open youtube" in command:
            speak("Opening YouTube")
            webbrowser.open("https://www.youtube.com")

        elif "open google" in command:
            speak("Opening Google")
            webbrowser.open("https://www.google.com")

        elif "open notepad" in command:
            speak("Opening Notepad")
            os.system("notepad.exe")

        elif "exit" in command or "stop" in command or "bye" in command:
            speak("Goodbye! Have a great day.")
            break

        elif command != "":
            speak("Sorry, I can only do basic tasks in offline mode.")

if __name__ == "__main__":
    main()
