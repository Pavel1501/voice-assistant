import os
import time 
import speech_recognition as sr
from fuzzywuzzy import fuzz
import pyttsx3
import datetime
opts = {
    "alias":('Jarvis', 'Jar', 'Jaaaaaaarviiis', 'jarvis'),
    "tbr":('tell', 'show', 'How much', 'say' ), 
    "cmds":{
        "ctime": ('What time is it now', 'what`s the time', 'Jarvis Whats the time'),
        "music": ('Turn the music on', 'turn on the music', 'Jarvis  music'),
        "lought": ('Tell the anecdote', 'do you know some anecdotes'),
        "tanks": ("tanks"),
        "google": ('google')

        


    }
}
# functions coul be deleted

def speak(what):
    print(what)
    speak_engine.say(what)
    speak_engine.runAndWait()
    speak_engine.stop()
    
def callback(recognizer, audio):
    try:
        voice = recognizer.recognize_google(audio, language = "eng-ENG").lower()
        print ("[log] Recognized:" +voice)
        cmd = voice
        if voice.startswith(opts["alias"]):
            cmd = voice
            for x in opts['alias']:
                cmd = cmd.replace(x, "").strip()
        
            for x in opts['tbr']:
                cmd = cmd.replace(x, "").strip()
        
        # recognise and perform a command
        cmd = recognize_cmd(cmd)
        execute_cmd(cmd['cmd'])
    except sr.UnknownValueError:
        print (" The voice was not recognised")
    except sr.RequestError as e:
        print(" Unknown Error, check your internet connection")
def recognize_cmd(cmd):
    RC = {'cmd': "", 'percent': 0}
    for c, v in opts['cmds'].items():
        for x in v:
            vrt = fuzz.ratio(cmd, x)
            if vrt > RC ['percent']:
                RC ['cmd'] = c
                RC['percent'] = vrt
            
    return RC
            
def execute_cmd(cmd):
    
    if cmd == 'ctime':
    # Tell the current time
    
        now = datetime.datetime.now()
        speak("Now" + " " + str(now.hour) + " " + "hours" +  ' ' + str(now.minute) +  " " + "minutes")
    
    elif cmd == "music":
    
        # Turn the music off
        os.system("D:\\МУЗЫКА\\11-50_cent-pimp-mp3engine.mp3")
        
    elif cmd == "tanks":
        os.system("D:\\GAMES\\WOT\\WorldOfTanks.exe")
        
    elif cmd == 'lought':
    
        speak ("Pasha did not teach me any anecdotes ")
    elif cmd == 'google':
        os.system('E:\\Google\\Chrome\\Application\\chrome.exe')

    
    else:
        print ('The command was not recognized')
    
#Turn on

r = sr.Recognizer()

m = sr. Microphone(device_index = 1)

with m as source:
    r.adjust_for_ambient_noise(source)

speak_engine = pyttsx3.init()




# if you uploaded some voices

voices = speak_engine.getProperty('voices')
speak('Hi, Sir, I am your virtual assistant Jarvis, I am listening')

stop_listening = r.listen_in_background(m, callback)
while True: time.sleep(0.1)







