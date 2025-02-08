pip install -U openai-whisper
import whisper
model = whisper.load_model("large")
result = model.transcribe("Educacao.mp3")
print(result['text'])
