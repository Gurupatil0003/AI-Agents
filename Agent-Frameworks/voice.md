# Voice to text


```python

import speech_recognition as sr

# Initialize recognizer
recognizer = sr.Recognizer()

# Use the microphone as source
with sr.Microphone() as source:
    print("🎙️ Speak something...")
    recognizer.adjust_for_ambient_noise(source)
    audio = recognizer.listen(source)

    try:
        # Recognize speech using Google's API
        text = recognizer.recognize_google(audio)
        print("📝 You said:", text)
    except sr.UnknownValueError:
        print("❌ Could not understand audio.")
    except sr.RequestError as e:
        print("🔌 Could not request results; check your internet connection.", e)




```




```python
# Install gTTS
!pip install gTTS

from gtts import gTTS
from IPython.display import Audio, display

def text_to_speech(text, filename="output.mp3", lang="en"):
    """
    Converts text to speech using gTTS and saves the audio file.
    """
    tts = gTTS(text=text, lang=lang, slow=False)
    tts.save(filename)
    return filename

# Usage
sample_text = "Hello Guru, how are you doing today?"
audio_file = text_to_speech(sample_text)

# Optional: play the audio
display(Audio(audio_file))


```

```python

# Install required libraries
!pip install SpeechRecognition pydub
!apt install ffmpeg  # Only needed for Colab/Linux

import speech_recognition as sr
from pydub import AudioSegment

def speech_to_text(filename):
    """
    Converts an MP3/WAV file to text using Google's Speech Recognition API.
    """
    # Convert MP3 to WAV if needed
    if filename.endswith('.mp3'):
        sound = AudioSegment.from_mp3(filename)
        wav_filename = filename.replace('.mp3', '.wav')
        sound.export(wav_filename, format="wav")
    else:
        wav_filename = filename

    recognizer = sr.Recognizer()
    with sr.AudioFile(wav_filename) as source:
        audio_data = recognizer.record(source)
        try:
            return recognizer.recognize_google(audio_data)
        except sr.UnknownValueError:
            return "Could not understand audio"
        except sr.RequestError:
            return "Speech recognition service unavailable"

# Usage
recognized_text = speech_to_text(audio_file)
print("Recognized Text:", recognized_text)


```


```
