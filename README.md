# Python-Programming
import speech_recognition as sr
import pyttsx3
from datetime import datetime

assistant_voice = pyttsx3.init()
assistant_voice.setProperty("rate", 165)  

def speak(text):
    """The assistant talks like a friendly human"""
    print(f"🤖 Assistant: {text}")
    assistant_voice.say(text)
    assistant_voice.runAndWait()

listener = sr.Recognizer()

def listen_to_user():
    """Listen patiently and try to understand what the user says"""
    with sr.Microphone() as mic:
        print("🎧 I'm listening... go ahead")
        listener.adjust_for_ambient_noise(mic, duration=0.6)

        try:
            audio = listener.listen(
                mic,
                timeout=6,
                phrase_time_limit=6
            )
        except sr.WaitTimeoutError:
            print("😶 I didn’t hear anything")
            return ""

    try:
        spoken_words = listener.recognize_google(audio)
        print(f"🗣️ You: {spoken_words}")
        return spoken_words.lower()

    except sr.UnknownValueError:
        speak("Hmm… I didn’t quite catch that. Could you say it again?")
        return ""

    except sr.RequestError:
        speak("I’m having trouble reaching the internet right now.")
        return ""

def run_assistant():
    speak("Hey there! I’m ready whenever you are.")

    while True:
        command = listen_to_user()

        if not command:
            continue

        if "hello" in command or "hi" in command:
            speak("Hello! It’s nice to hear your voice.")

        elif "time" in command:
            current_time = datetime.now().strftime("%I:%M %p")
            speak(f"It’s {current_time} right now.")

        elif "date" in command:
            today = datetime.now().strftime("%d %B %Y")
            speak(f"Today is {today}.")

        elif "how are you" in command:
            speak("I’m doing great, thanks for asking! How about you?")

        elif "exit" in command or "quit" in command or "bye" in command:
            speak("Alright, take care. Talk to you soon!")
            break

        else:
            speak("I heard you, but I’m not sure how to help with that yet.")

if __name__ == "__main__":
    run_assistant()
