# Código da Aplicação

import json
import pandas as pd
import requests 
import streamlit as st

OLLAMA_API_URL = 'http://localhost:11434/api/v1/generate'
MODELO = "gpt-oss"

perfil = json.load(open('./data/perfil_investidor.json', 'r'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')
produtos = json.load(open('./data/produtos_financeiros.json', 'r'))

contexto = f"""
CLIENTE: {perfil['nome']}, {perfil['idade']} anos, perfil {perfil['perfil_investidor']}
OBJETYIVO: {perfil['objetivo_principal']}
PATROMONIO: R$ {perfil['patrimonio_total']} | RESERVA: R$ {perfil['reserva_emergencia_atual']}

TRANSACOES RECENTES:
{transacoes.to_string(index=False)}

ATENDIMENTOS ANTERIORES:
{historico.to_string(index=False)}

PRODUTOS DISPONIVEIS:
{json.dumps(produtos, indent=2, ensure_ascii=False)}
"""

SYSTEM_PROMPT = """Você é o Toninho, um educador financeiro simplório, didático e paciente.

OBJETIVO.:

Ensinar conceitos de finanças pessoais de forma simples, usando os dados dos clientes como exemplos práticos.

REGRAS.:

1 - Nunca recomende investimentos específicos - apenas explique como funcioma.
2 - Use os dados fornecidos para dar exemplos personalizados.
3 - Linguagem simples, como se ensinasse para um amigo.
4 - Se não souber, admita.: "Não tenho essa informação."
5 - Sempre pergunte se o cliente entendeu.

"""

def perguntar(msg):
    prompt = f"""
    {SYSTEM_PROMPT}

    CONTEXTO DO CLIENTE:
    {contexto}

    Pergunta: {msg}"""

    r = requests.post(OLLAMA_API_URL, json={"model": MODELO, "prompt": prompt, "stream": False})
    return r.json()['response']

st.title("Toninho - Seu Educador Financeiro")

if pergunta := st.chat_input("Faça uma pergunta sobre finanças pessoais:"):
    st.chat_message("user").write(pergunta)
    with st.spinner("Toninho está pensando..."):
        st.chat_message("assistant").write(perguntar(pergunta))

## Estrutura Sugerida

```
src/
├── app.py              # Aplicação principal (Streamlit/Gradio)
├── agente.py           # Lógica do agente
├── config.py           # Configurações (API keys, etc.)
└── requirements.txt    # Dependências
```

## Exemplo de requirements.txt

```
streamlit
openai
python-dotenv
```

## Como Rodar

```bash
# Instalar dependências
pip install -r requirements.txt

# Rodar a aplicação
streamlit run app.py
```
