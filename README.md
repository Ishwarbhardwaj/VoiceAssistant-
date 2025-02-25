# VoiceAssistant-
"Exploring the power of voice assistants! 🚀 Integrating AI-driven voice technology to enhance user interactions and automate tasks. Check out the repo for insights, code samples, and contributions! #CodeAlpha#VoiceAssistant #OpenSource"
import speech_recognition as sr
import pyttsx3
import datetime
import pywhatkit as kit
import webbrowser
import os

# Initialize the recognizer and the text-to-speech engine
recognizer = sr.Recognizer()
engine = pyttsx3.init()

# Dictionary to map specific song names to YouTube URLs
specific_songs = {
    "despacito": "https://www.youtube.com/watch?v=kJQP7kiw5Fk",
    "shape of you": "https://www.youtube.com/watch?v=JGwWNGJdvx8",
    "blinding lights": "https://www.youtube.com/watch?v=4NRXx6U8ABQ",
    "see you again": "https://www.youtube.com/watch?v=RgKAFK5djSk",
    "perfect": "https://www.youtube.com/watch?v=2Vv-BfVoq4g",
    "someone you loved": "https://www.youtube.com/watch?v=zABLecsR5UE",
    "bad guy": "https://www.youtube.com/watch?v=DyDfgMOUjCI",
    "hanuman chalisa": "https://www.youtube.com/watch?v=srNoYnGhXAg"  
      # Add more specific songs and their URLs here
}

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)
        try:
            command = recognizer.recognize_google(audio)
            print(f"You said: {command}")
            return command.lower()
        except sr.UnknownValueError:
            print("Sorry, I did not understand that.")
            return ""
        except sr.RequestError:
            print("Could not request results from Google Speech Recognition service.")
            return ""

def respond_to_command(command):
    if "time" in command:
        current_time = datetime.datetime.now().strftime("%H:%M")
        speak(f"The current time is {current_time}.")
    elif "search" in command:
        query = command.replace("search", "").strip()
        speak(f"Searching for {query}.")
        kit.search(query)
    elif "play" in command:
        song = command.replace("play", "").strip()
        if song in specific_songs:
            speak(f"Playing {song}.")
            webbrowser.open(specific_songs[song])
        else:
            speak(f"Playing {song} on YouTube.")
            kit.playonyt(song)
    elif "open youtube" in command:
        speak("Opening YouTube.")
        webbrowser.open("https://www.youtube.com")
    elif "open google" in command:
        speak("Opening Google.")
        webbrowser.open("https://www.google.com")
    elif "open camera" in command:
        speak("Opening camera.")
        # This command may vary based on your operating system
        os.system("start microsoft.windows.camera:")  # For Windows
        # For Mac, you might use: os.system("open /Applications/Photo Booth.app")
    elif "open chat gpt" in command:
        speak("Opening ChatGPT.")
        webbrowser.open("https://chat.openai.com")
    elif "hello" in command:
        speak("Hello! I am Aku, your voice assistant. How can I assist you today?")
    elif "exit" in command or "quit" in command:
        speak("Goodbye! thank you for using me. Have a great day!")
        exit()
    else:
        speak("I'm sorry, I can't help with that.")

def main():
    speak("Hello! I am Aku, your voice assistant. How can I help you?")
    while True:
        command = listen()
        respond_to_command(command)

if __name__ == "__main__":
    main()
