# EDO
tutor para aprendizado de EDO
# Instalando o SDK do Google!
!pip install -q -U google-generativeai

# Configurações iniciais
import google.generativeai as genai
import textwrap
from IPython.display import display
from IPython.display import Markdown

# Configurando a chave de API do Google
GOOGLE_API_KEY = ""
genai.configure(api_key=GOOGLE_API_KEY)

# Listando os modelos disponíveis
for model in genai.list_models():
  if 'generateContent' in model.supported_generation_methods:
    print(model.name)

# Configurações do modelo
generation_config = {
  "candidate_count": 1,
  "temperature": 0.7, # Ajuste para controlar a criatividade
}

# Definindo as configurações de segurança
safety_settings = {
    'HATE': 'BLOCK_NONE',
    'HARASSMENT': 'BLOCK_NONE',
    'SEXUAL': 'BLOCK_NONE',
    'DANGEROUS': 'BLOCK_NONE'
}

# Criando um objeto do modelo generativo
model = genai.GenerativeModel(
    model_name='gemini-1.0-pro',
    generation_config={
        "candidate_count": 1,
        "temperature": 0.7,  # Ajuste para controlar a criatividade
    },
    safety_settings=safety_settings
)

# Função para formatar a saída
def to_markdown(text):
  text = text.replace('•', '  *')
  return Markdown(textwrap.indent(text, '> ', predicate=lambda _: True))

# Iniciando o chat
chat = model.start_chat(history=[])
print("Olá! Sou seu tutor de EDOs. O que você gostaria de saber?")
prompt = input('> ')

while prompt.lower() != "fim":
  response = chat.send_message(prompt)
  display(to_markdown(f'**Tutor**: {response.text}'))  
  print('-------------------------------------------')
  prompt = input('> ')

print("Até a próxima! Bons estudos.")
