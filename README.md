# voice_assist
import speech_recognition as sr
from gtts import gTTS
import os

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
        try:
            return recognizer.recognize_google(audio)
        except sr.UnknownValueError:
            return "Could not understand audio"
        except sr.RequestError as e:
            return f"Could not request results from Google Speech Recognition service; {e}"

def speak(text):
    tts = gTTS(text=text, lang='en')
    tts.save("response.mp3")
    os.system("start response.mp3")

if _name_ == "_main_":
    while True:
        command = listen().lower()
        print("You said:", command)

        if "hello" in command:
            speak("Hi there! How can I help you?")
        elif "goodbye" in command:
            speak("Goodbye! Have a great day!")
            break
        else:
            speak("I didn't understand that. Could you please repeat?")
