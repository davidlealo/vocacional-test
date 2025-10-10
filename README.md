# Azure OpenAI – Vocacional Test

Repositorio de pruebas con **Azure AI Foundry (OpenAI Service)** para conectar, desplegar y probar modelos como `Phi-4-mini-instruct` mediante llamadas seguras desde Google Colab o entorno local.

---

## Objetivo

Este repositorio permite:
- Conectarse a un **deployment de Azure OpenAI** sin exponer claves en el código.  
- Probar inferencias con modelos como **Phi-4-mini**, **GPT-4-turbo**, etc.  
- Guardar las pruebas realizadas para futuros proyectos (por ejemplo, *VocacionalTest*, *MentorIA*, etc.).

---

## ⚙️ Configuración Inicial

1. **Clonar el repositorio:**

   ```bash
   git clone https://github.com/tuusuario/azure-openai-tests.git
   cd azure-openai-tests
   ```

2. **Instalar dependencias:**

   Si trabajas localmente:

   ```bash
   pip install -r requirements.txt
   ```

   Si usas **Google Colab**, solo ejecuta en la primera celda del notebook:

   ```python
   !pip install -r requirements.txt
   ```

3. **Crear un archivo `.env` (no lo subas a GitHub)** con tus credenciales de Azure OpenAI:

   ```
   AZURE_OPENAI_KEY=tu_api_key_aqui
   AZURE_OPENAI_ENDPOINT=https://oai-vocatest.services.ai.azure.com/
   AZURE_OPENAI_DEPLOYMENT=phi4mini-chat
   ```

4. **(Opcional)** Agrega un `.gitignore` con:
   ```
   .env
   __pycache__/
   .ipynb_checkpoints/
   ```

---

## 💻 Ejecución del Notebook

1. Abre el notebook `notebooks/vocacional-test.ipynb` en Google Colab.  
2. Carga las variables de entorno con:

   ```python
   from dotenv import load_dotenv
   load_dotenv()
   ```

3. Ejecuta el bloque de prueba REST:

   ```python
   import os, requests, json

   API_KEY = os.getenv("AZURE_OPENAI_KEY")
   ENDPOINT = os.getenv("AZURE_OPENAI_ENDPOINT")
   DEPLOYMENT = os.getenv("AZURE_OPENAI_DEPLOYMENT")

   URL = f"{ENDPOINT}openai/deployments/{DEPLOYMENT}/chat/completions?api-version=2024-05-01-preview"

   headers = {"Content-Type": "application/json", "api-key": API_KEY}
   data = {
       "messages": [
           {"role": "system", "content": "Eres un asistente educativo amable."},
           {"role": "user", "content": "Explica qué es el aprendizaje automático con un ejemplo."}
       ],
       "temperature": 0.7,
       "max_tokens": 300
   }

   response = requests.post(URL, headers=headers, data=json.dumps(data))
   print(response.json()["choices"][0]["message"]["content"])
   ```

4. Si ves una respuesta del modelo, ¡tu implementación está lista! 

---

## 🔒 Seguridad

- Las claves y endpoints se leen desde variables de entorno (`.env`), **no se deben imprimir ni subir al repo**.
- El notebook incluye manejo seguro de `getpass()` para ingresar la clave si no está definida.

---

## Estructura del proyecto

```
azure-openai-tests/
│
├── notebooks/
│   └── vocacional-test.ipynb
│
├── .env               # 🔒 (no subir)
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Próximos pasos

- Crear notebooks adicionales para pruebas con:
  - **Mistral-7B / Mixtral**
  - **GPT-4-turbo / GPT-4o**
  - **Modelos de embeddings**
- Integrar funciones en un backend (FastAPI o Flask) para consumo desde una app Flutter o React.

---

## 🧑‍💻 Autor

**David Leal Olivares**  
ONG Innovacien | Proyectos Educativos y IA  
📧 [david@innovacien.org](mailto:david@innovacien.org)
