# 🤖 Chatbot Sprint 3

Chatbot de FAQ em Python/Flask que combina busca por palavra-chave com um modelo de NLP (Hugging Face `transformers`) para responder perguntas com base em uma base de conhecimento (`faq.json`), com sugestões de perguntas em tempo real.

**Autor:** Lucas Carabolad Bob — [@lucasrabd](https://github.com/lucasrabd)

---

## 📦 Stack

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat&logo=flask&logoColor=white)
![Transformers](https://img.shields.io/badge/🤗%20Transformers-DistilBERT-FFD21E?style=flat)
![HTML](https://img.shields.io/badge/HTML5-CSS3-E34F26?style=flat&logo=html5&logoColor=white)

---

## 🧠 Como funciona

1. **Busca por palavra-chave** — ao receber uma pergunta, o `ChatBot` primeiro procura uma correspondência exata ou parcial (case-insensitive) entre as perguntas cadastradas em `faq.json`.
2. **Fallback com NLP** — se não houver correspondência direta, entra em ação um pipeline `question-answering` do Hugging Face (`distilbert-base-uncased`), que avalia cada FAQ como contexto e retorna a resposta com maior *score* de confiança (limiar mínimo de `0.5`).
3. **Sugestões em tempo real** — enquanto o usuário digita, o endpoint `/sugestoes` retorna perguntas da base que contêm o texto parcial digitado, exibidas como chips clicáveis na interface.

---

## 📁 Estrutura do Projeto

```
Chatbot-Sprint-3-main/
├── app.py            # Rotas Flask (/, /pergunta, /sugestoes)
├── chatbot.py         # Classe ChatBot — busca por palavra-chave + pipeline NLP
├── faq.json            # Base de perguntas e respostas (FAQ)
├── templates/
│   └── index.html       # Interface de chat (HTML + CSS + JS embutidos)
└── static/
    └── styles.css         # Folha de estilo alternativa (não referenciada em index.html)
```

> ⚠️ **`static/styles.css` não é usado:** `templates/index.html` define seus próprios estilos em uma tag `<style>` inline; o arquivo CSS em `static/` está órfão. Se quiser usá-lo, adicione `<link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">` ao `<head>` do template.
>
> ⚠️ **Sem `requirements.txt`:** o projeto não inclui um arquivo de dependências — veja a seção [Instalação](#-instalação) para os pacotes necessários.

---

## 🚀 Instalação

### Pré-requisitos
- Python 3.x
- pip

### Passos

1. Clone o repositório:
```bash
git clone https://github.com/lucasrabd/Chatbot-Sprint-3.git
cd Chatbot-Sprint-3
```

2. Crie um ambiente virtual (recomendado):
```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```

3. Instale as dependências:
```bash
pip install flask transformers torch
```

4. Execute a aplicação:
```bash
python app.py
```

5. Acesse `http://localhost:5000` no navegador.

> A primeira execução baixa os pesos do modelo `distilbert-base-uncased` do Hugging Face Hub (requer conexão com a internet).

---

## 🔌 Rotas da API

| Método | Rota | Body (JSON) | Descrição |
|---|---|---|---|
| GET | `/` | — | Renderiza a interface do chat (`index.html`) |
| POST | `/pergunta` | `{ "question": "..." }` | Retorna a resposta do chatbot para a pergunta enviada |
| POST | `/sugestoes` | `{ "texto": "..." }` | Retorna lista de perguntas da FAQ que contêm o texto digitado |

### Exemplo — perguntar

```bash
curl -X POST http://localhost:5000/pergunta \
  -H "Content-Type: application/json" \
  -d '{"question": "O que é IoT?"}'
```

```json
{ "response": "IoT é a Internet das Coisas, que permite a comunicação entre dispositivos pela internet." }
```

### Exemplo — sugestões

```bash
curl -X POST http://localhost:5000/sugestoes \
  -H "Content-Type: application/json" \
  -d '{"texto": "intelig"}'
```

```json
{ "sugestoes": ["O que é inteligência artificial?", "Quais são as aplicações da inteligência artificial?"] }
```

---

## 📚 Base de Conhecimento (`faq.json`)

Atualmente cobre 8 perguntas sobre IoT, Inteligência Artificial e Machine Learning (o que é IoT, benefícios da IoT, o que é IA, aprendizado supervisionado, linguagens usadas em IA, etc.). Para adicionar novas entradas, edite o array `faq` seguindo o formato:

```json
{ "question": "Sua pergunta aqui?", "answer": "Resposta correspondente." }
```

---

## 📄 Licença

Não especificada.
