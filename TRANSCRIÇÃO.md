# Instalar dependências necessárias (se ainda não estiverem instaladas)
!pip install langchain langchain-google-genai openai-whisper

# Importar as bibliotecas
from langchain.prompts import PromptTemplate
from langchain_google_genai.llms.llms import GoogleGenerativeAI
from whisper import load_model

# Funções auxiliares para carregar configurações (substituir por seus próprios valores)
def get_configs():
    return {
        "llm_model": {
            "name": "gemini-pro",  # Modelo do Google Generative AI
            "temperature": 0.7,
            "top_p": 0.9,
            "top_k": 40
        },
        "transcription_model": "large"  # Modelo Whisper
    }

def get_llm_model_api_key(provider: str):
    # Substituir pela chave de API válida
    return "AIzaSyDQvP4cdQCh2ZxUCP546DsEnLHpXOS7TYo"

# Carregar configurações
configs = get_configs()
api_key = get_llm_model_api_key("Google")

# Configurar o modelo de IA do Google Generative AI
llm_model = GoogleGenerativeAI(
    model=configs["llm_model"]["name"],
    api_key=api_key,
    temperature=configs["llm_model"]["temperature"],
    top_p=configs["llm_model"]["top_p"],
    top_k=configs["llm_model"]["top_k"]
)

# Criar o template do prompt
template = """
    Resume a text from an audio transcription and extract its key words.
    Resume what was said, and what the audio text transcript is about.
    Structure these in the below form with "resume:" and "key words:" sections and
    don't put the original text in the output.
    If you see any "[FAKE AUDIO]" in the start of the text, just ignore it, but do the tasks with the text.

    TEXT: {text}

    resume:

    key words:
"""

prompt_template = PromptTemplate(
    input_variables=["text"],
    template=template
)

# Função para resumir um texto
def resume_text(text: str) -> str:
    """
    Resume o texto e extrai palavras-chave usando um modelo de IA.
    """
    chain = prompt_template | llm_model
    response = chain.invoke({"text": text})

    print(response)

# Função para transcrever um áudio
def transcribe_audio(audio_path: str) -> str:
    """
    Transcreve o áudio dado um caminho de arquivo.
    """
    if not audio_path.endswith((".mp3", ".wav")):
        raise ValueError("Apenas arquivos .mp3 e .wav são suportados.")

    w_model = load_model(configs["transcription_model"])
    return w_model.transcribe(audio_path, fp16=False)["text"]

# Exemplo de uso
audio_path = "Educacao.mp3"  # Substitua pelo caminho correto do arquivo
try:
    transcribed_text = transcribe_audio(audio_path)
    resume_text(transcribed_text)
except Exception as e:
    print("Erro:", e)
