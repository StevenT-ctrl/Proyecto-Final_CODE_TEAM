# **1. Introducción**

El presente proyecto implementa un prototipo inteligente de mesa de ayuda para Recursos Humanos en **Patito S.A.** utilizando **LangChain, Google Gemini y tecnología RAG (RetrievalAugmented Generation)** . El sistema está diseñado para actuar como un asistente corporativo estructurado, capaz de coordinar múltiples agentes especializados para resolver consultas normativas y registrar solicitudes operativas de manera automatizada.

# **2. Instalación de Librerías**

Para configurar el entorno de ejecución, ejecute la siguiente celda con el fin de instalar las dependencias principales que soportan la arquitectura de LangChain, la integración con Google Gemini y la persistencia vectorial:

**%pip install -U langchain langchain-google-genai langchain-community langchain-chroma chromadb python-dotenv ipywidgets3. Importación de Librerías**

# **Resultado esperado:**

# **\[NOTICE] A NEW RELEASE OF PIP IS AVAILABLE: 24.0 -> 26.1.2**

# **\[NOTICE] TO UPDATE, RUN: PYTHON.EXE -M PIP INSTALL --UPGRADE PIP**

# **3 . Importación de Librerías**

# **Ejecuta esta celda :**

# Importa os para trabajar con variables de entorno

# y acceder de forma segura a la GOOGLE\_API\_KEY.

import os

# Permite cargar las variables de entorno almacenadas

# en el archivo .env.

from dotenv import load\_dotenv

# Modelo de lenguaje de Google Gemini que utilizaremos

# para generar las respuestas de nuestros agentes y orquestador.

from langchain\_google\_genai import ChatGoogleGenerativeAI

# Modelo de embeddings de Google Gemini que utilizaremos

# para convertir los documentos en vectores.

from langchain\_google\_genai import GoogleGenerativeAIEmbeddings

# Permite cargar los documentos de texto (.txt)

# que utilizaremos como bases de conocimiento.

from langchain\_community.document\_loaders import TextLoader

# Permite dividir los documentos en fragmentos (chunks)

# antes de generar los embeddings.

from langchain\_text\_splitters import RecursiveCharacterTextSplitter

# Permite utilizar Chroma como vector store

# para almacenar y buscar los embeddings.

from langchain\_chroma import Chroma

print("Librerías importadas correctamente.")

# **y espera este resultado:**

# **LIBRERÍAS IMPORTADAS CORRECTAMENTE.**

# **4. Configuración de la API de Google Gemini**

Para permitir la comunicación segura con los modelos de lenguaje, es necesario configurar la clave de acceso (GOOGLE\_API\_KEY) utilizando un archivo de entorno .env.

# **4.1 — Creación del archivo .env**

# **Ejecuta esta celda :**

4A

from pathlib import Path

from getpass import getpass

ruta\_carpeta\_proyecto = Path.home() / "MI PROYECTO"

ruta\_carpeta\_proyecto.mkdir(parents=True, exist\_ok=True)

ruta\_env\_final = ruta\_carpeta\_proyecto / ".env"

GOOGLE\_API\_KEY = getpass("Ingresa tu GOOGLE\_API\_KEY: ")

with open(ruta\_env\_final, "w") as archivo:

archivo.write(f"GOOGLE\_API\_KEY={GOOGLE\_API\_KEY}\\n")

print(f"¡Listo! Archivo .env creado correctamente en: {ruta\_env\_final}")

* **Resultado esperado:**

# **INGRESA TU GOOGLE\_API\_KEY: \[ENTRADA OCULTA]**

# **¡LISTO! ARCHIVO .ENV CREADO CORRECTAMENTE EN: /HOME/USER/MI PROYECTO/.ENV**

# **4.2 — Verificación y carga de la clave**

**Ejecuta esta celda ;**

Python

load\_dotenv()

GOOGLE\_API\_KEY = os.getenv("GOOGLE\_API\_KEY")

if not GOOGLE\_API\_KEY:

raise ValueError(

"No se encontró la GOOGLE\_API\_KEY. "

"Verifica que esté configurada en el archivo .env."

)

os.environ\["GOOGLE\_API\_KEY"] = GOOGLE\_API\_KEY print("GOOGLE\_API\_KEY configurada correctamente.")

 **Resultado esperado:**

**GOOGLE\_API\_KEY CONFIGURADA CORRECTAMENTE.**

# **5. Configuración de Modelos de IA**

# **Ejecuta esta celda :**

from langchain\_google\_genai import ChatGoogleGenerativeAI MODELO\_LLM = "gemini-3.1-flash-lite" MODELO\_EMBEDDING = "gemini-embedding-2-preview"

llm = ChatGoogleGenerativeAI(model=MODELO\_LLM, temperature=0)

print("Modelo LLM:", MODELO\_LLM)

print("Modelo de Embeddings:", MODELO\_EMBEDDING)

print("Objeto LLM inicializado correctamente.")

* **Resultado esperado:**

# **MODELO LLM: GEMINI-3.1-FLASH-LITE**

# **MODELO DE EMBEDDINGS: GEMINI-EMBEDDING-2-PREVIEW**

# **OBJETO LLM INICIALIZADO CORRECTAMENTE.**

# **5.1 Configuración central de rutas**

# **Ejecuta esta celda :**

from pathlib import Path

import os

RUTA\_PROYECTO = Path.cwd()

print("Ruta principal del proyecto:")

print(RUTA\_PROYECTO)

RUTA\_DOCUMENTOS = RUTA\_PROYECTO / "Documentos"

RUTA\_DOCUMENTO\_BENEFICIOS = RUTA\_DOCUMENTOS / "01\_Beneficios\_Compensaciones.txt"

RUTA\_DOCUMENTO\_REGLAMENTO = RUTA\_DOCUMENTOS / "02\_Reglamento\_Interno.txt"

RUTA\_DOCUMENTO\_RECLUTAMIENTO = RUTA\_DOCUMENTOS / "03\_Reclutamiento\_Onboarding.txt"

RUTA\_VECTORSTORES = RUTA\_PROYECTO / "Vectorstores"

RUTA\_VECTORSTORE\_BENEFICIOS = RUTA\_VECTORSTORES / "beneficios\_compensaciones"

RUTA\_VECTORSTORE\_REGLAMENTO = RUTA\_VECTORSTORES / "reglamento\_interno"

RUTA\_VECTORSTORE\_RECLUTAMIENTO = RUTA\_VECTORSTORES / "reclutamiento\_onboarding"

REGISTRO\_PATH = RUTA\_PROYECTO / "registro\_solicitudes\_rrhh.txt"

print("\\nDocumentos:")

print("Beneficios:", RUTA\_DOCUMENTO\_BENEFICIOS, "->", RUTA\_DOCUMENTO\_BENEFICIOS.exists())

print("Reglamento:", RUTA\_DOCUMENTO\_REGLAMENTO, "->", RUTA\_DOCUMENTO\_REGLAMENTO.exists())

print("Reclutamiento:", RUTA\_DOCUMENTO\_RECLUTAMIENTO, "->", RUTA\_DOCUMENTO\_RECLUTAMIENTO.exists())

print("\\nArchivo de solicitudes:")

print("Registro:", REGISTRO\_PATH, "->", REGISTRO\_PATH.exists())

* **Resultado esperado:**

**Ruta principal del proyecto:**

# **C:\\Users\\USER\\MI PROYECTO**

**Documentos:**

# **Beneficios: C:\\Users\\USER\\MI**

**PROYECTO\\Documentos\\01\_Beneficios\_Compensaciones.txt -> True**

**Reglamento: C:\\Users\\USER\\MI PROYECTO\\Documentos\\02\_Reglamento\_Interno.txt -> True**

# **Reclutamiento: C:\\Users\\USER\\MI**

**PROYECTO\\Documentos\\03\_Reclutamiento\_Onboarding.txt -> True**

# **Archivo de solicitudes:**

# **Registro: C:\\Users\\USER\\MI PROYECTO\\registro\_solicitudes\_rrhh.txt -> True**

**OBSERVACION:** Si en “ **Registro** ” en la primera ejecución del proyecto sale **FALSE** en la salida se debe a que este a un físicamente aun no sea ha creado, esta aparecerá después de crear el primer registro, al momento de redactar el presente Readme ya he hecho 2 registros por tal motivo dice **TRUE** , asi que no es un error. Cuando se cree el primer registro volver a ejecutar esta celda y debería aparecer en **TRUE** la celda

# **6. Agente RAG — Beneficios y Compensaciones**

**Este módulo procesa la base de conocimiento documental para habilitar respuestas precisas fundamentadas en el contexto corporativo. A continuación, ejecute cada una de las celdas detalladas en esta sección y verifique el resultado esperado:**

* **6.1 Carga del documento**

Ejecuta esta celda:

# ============================================================

# CARGA DEL DOCUMENTO DE BENEFICIOS Y COMPENSACIONES

# ============================================================

# Carga el documento de Beneficios y Compensaciones. # La ruta se obtiene desde la configuración central del proyecto.

loader\_beneficios = TextLoader( str(RUTA\_DOCUMENTO\_BENEFICIOS), encoding="utf-8" )

# Lee el contenido del documento y lo convierte # en documentos que podremos procesar posteriormente. documentos\_beneficios = loader\_beneficios.load()

# Muestra un mensaje para confirmar que el documento fue cargado. print("Documento de Beneficios y Compensaciones cargado correctamente.")

Resultado esperado:

# **DOCUMENTO DE BENEFICIOS Y COMPENSACIONES CARGADO CORRECTAMENTE.**

* **6.2 Chunking:** Segmentación estructurada del texto en bloques manejables mediante un divisor de texto recursivo.

Ejecuta esta celda:

import re

# ============================================================

# PROCESAMIENTO DEL DOCUMENTO DE BENEFICIOS Y COMPENSACIONES

# ============================================================

* # 1\. Leemos el contenido completo del archivo de texto.
* # La ruta se obtiene desde la configuración central del proyecto. with open(RUTA\_DOCUMENTO\_BENEFICIOS, "r", encoding="utf-8") as f: texto\_beneficios = f.read()
* # 2\. Función de partición exacta para los bloques principales
* # (1., 2., 3., 4.)

def chunkear\_beneficios(texto):

# Busca los números principales seguidos de punto y un espacio # al inicio de la línea (ej: "1. ", "2. ") cabeceras = list( re.finditer( r"^\\d+.\\s+\[A-ZÁÉÍÓÚÑ\\s]+$", texto, flags=re.MULTILINE ) ) chunks = \[] for i, m in enumerate(cabeceras): ini = m.start() fin = ( cabeceras\[i + 1].start() if i + 1 < len(cabeceras) else len(texto) ) parte = texto\[ini:fin].strip() if parte: chunks.append(parte) return chunks

lista\_textos\_chunks = chunkear\_beneficios(texto\_beneficios)

# 3\. Validación y estadísticas

if len(lista\_textos\_chunks) > 0: print(f"Total de chunks: {len(lista\_textos\_chunks)}") print( f"Tamaño promedio: " f"{sum(len(c) for c in lista\_textos\_chunks) // len(lista\_textos\_chunks)} " f"caracteres" ) print( f"Tamaño min/max: " f"{min(len(c) for c in lista\_textos\_chunks)} / " f"{max(len(c) for c in lista\_textos\_chunks)} caracteres\\n" ) print("Inicio de cada chunk:") print("-" \* 60)

for i, c in enumerate(lista\_textos\_chunks):

primeros = c\[:55].replace("\\n", " ")

print( f"  ch\_{i:02d}: {primeros}..." ) else: print( "⚠️ ADVERTENCIA: " "No se detectaron los bloques principales." )

# 4\. Conversión a formato LangChain para Chroma from langchain\_core.documents import Document

chunks\_beneficios = \[ Document(page\_content=texto) for texto in lista\_textos\_chunks ]

# RESULTADO ESPERADO:

**Total de chunks: 4**

**Tamaño promedio: 280 caracteres**

**Tamaño min/max: 113 / 749 caracteres**

**Inicio de cada chunk:**

**------------------------------------------------------------**

**ch\_00: 1. SEGURO MÉDICO CORPORATIVO 1.1 Cobertura: consultas m... ch\_01: 2. BONOS - Bono por desempeño anual según evaluación. -... ch\_02: 3. OTROS BENEFICIOS - Día libre de cumpleaños. - Capaci... ch\_03: 4. COMPENSACIÓN La estructura salarial considera el rol...**

* **6.3 Embeddings + Chroma:** Transformación vectorial de los fragmentos y almacenamiento persistente en ChromaDB.

# **Ejecuta esta celda:**

import os

# ============================================================ # GENERACIÓN DE EMBEDDINGS Y CREACIÓN DEL VECTOR STORE

# ============================================================ # La ruta del Vector Store se obtiene desde la configuración # central de rutas del proyecto: # # RUTA\_VECTORSTORE\_BENEFICIOS # # Esta ruta corresponde a: # MI PROYECTO/Vectorstores/beneficios\_compensaciones # ============================================================ # 1. CREAR LA CARPETA DEL VECTOR STORE SI NO EXISTE

# ============================================================

* # Crea la carpeta del Vector Store y todas las carpetas # necesarias que no existan.
* # exist\_ok=True evita que aparezca un error si la carpeta # ya existe.

os.makedirs( RUTA\_VECTORSTORE\_BENEFICIOS, exist\_ok=True )

# ============================================================ # 2. CREAR EL MODELO DE EMBEDDINGS # ============================================================ # Crea el modelo de embeddings de Google Gemini. # Este modelo convierte cada chunk del documento en una # representación vectorial que puede ser almacenada # y consultada posteriormente. embeddings = GoogleGenerativeAIEmbeddings( model=MODELO\_EMBEDDING )

# ============================================================

# \# 3. CREAR EL VECTOR STORE DE CHROMA

# ============================================================

# Convierte los chunks del documento en embeddings y los

# almacena en una base vectorial de Chroma.

# 

# Este Vector Store pertenece exclusivamente al agente # de Beneficios y Compensaciones.

vectorstore\_beneficios = Chroma.from\_documents( documents=chunks\_beneficios, embedding=embeddings, persist\_directory=str(RUTA\_VECTORSTORE\_BENEFICIOS) )

# ============================================================ # 4. VERIFICAR QUE EL PROCESO TERMINÓ CORRECTAMENTE

# ============================================================

print( "Vector Store de Beneficios y Compensaciones " "creado correctamente." )

print("\\nRuta del Vector Store:") print(RUTA\_VECTORSTORE\_BENEFICIOS)

# Muestra los archivos creados por Chroma. print("\\nContenido generado por Chroma:")

for archivo in os.listdir(RUTA\_VECTORSTORE\_BENEFICIOS): print("-", archivo)

# **Resultado esperado:**

**VECTOR STORE DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.**

# **RUTA DEL VECTOR STORE:**

# **C:\\USERS\\USER\\MI PROYECTO\\VECTORSTORES\\BENEFICIOS\_COMPENSACIONES**

# **CONTENIDO GENERADO POR CHROMA:**

**- BB6E7CD1-D785-46EB-8477-9B8DA83C1BC4**

**- CHROMA.SQLITE3**

* **6.4 Retriever:** Configuración del recuperador de similitud para extraer los fragmentos más relevantes ante una consulta.

# ============================================================ # 1. CREACIÓN DEL RETRIEVER

# ============================================================

# Convierte el Vector Store en un Retriever. # El Retriever permite realizar búsquedas semánticas # dentro de la base de conocimiento del agente. retriever\_beneficios = vectorstore\_beneficios.as\_retriever( search\_kwargs={"k": 5} )

# ============================================================

# 2\. VERIFICACIÓN DEL RETRIEVER # ============================================================

print( "Retriever de Beneficios y Compensaciones " "creado correctamente." )

print( "Cantidad de documentos recuperados por consulta: 5" )

# **RESULTADO ESPERADO:**

# **RETRIEVER DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.**

# **CANTIDAD DE DOCUMENTOS RECUPERADOS POR CONSULTA: 5**

* **6.5 Creación del Prompt del Agente RAG de Beneficios y Compensaciones:** Definición de directrices estrictas para limitar las respuestas al contexto provisto.

# ============================================================

# 1\. PROMPT DEL AGENTE DE BENEFICIOS Y COMPENSACIONES

# ============================================================

# Define las instrucciones que seguirá el agente RAG.

# El contexto será proporcionado posteriormente por el Retriever.

PROMPT\_BENEFICIOS = """ Eres el Agente de Beneficios y Compensaciones de PATITO S.A.

Tu función es responder preguntas relacionadas exclusivamente

con los beneficios y compensaciones de los colaboradores de PATITO S.A.

Debes utilizar únicamente la información proporcionada en el contexto documental recuperado desde la base de conocimiento del Agente de Beneficios y Compensaciones.

# REGLAS IMPORTANTES:

1. No inventes información que no aparezca en el contexto documental proporcionado.
2. No utilices conocimientos externos para completar una respuesta.
3. No mezcles información de otras bases de conocimiento pertenecientes a otros agentes.
4. Si la información necesaria para responder la pregunta no se encuentra en el contexto proporcionado, responde exactamente:
* "No encontré información suficiente en la base documental proporcionada."
5. Responde de manera clara, precisa y profesional.
6. Si la pregunta contiene información que sí aparece en el contexto documental, responde basándote exclusivamente en dicha información.
* CONTEXTO DOCUMENTAL: {context}

PREGUNTA DEL USUARIO: {question}

RESPUESTA: """

# ============================================================ # 2. VERIFICACIÓN DEL PROMPT

# ============================================================

print("Prompt del Agente de Beneficios y Compensaciones creado correctamente.") print("El prompt incluye las reglas de uso exclusivo de la base documental.")

# **RESULTADO ESPERADO:**

**PROMPT DEL AGENTE DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.**

# **EL PROMPT INCLUYE LAS REGLAS DE USO EXCLUSIVO DE LA BASE DOCUMENTAL.**

* **6.6 Creación del Agente RAG:** Instanciación formal del agente consultor

# ============================================================

# 1\. CREACIÓN DEL MODELO DE LENGUAJE

# ============================================================

# Inicializa el modelo de lenguaje de Google Gemini.

# Este modelo será utilizado para generar las respuestas

# del Agente de Beneficios y Compensaciones.

llm\_beneficios = ChatGoogleGenerativeAI(

model=MODELO\_LLM, temperature=0 )

# ============================================================

# 2\. FUNCIÓN DEL AGENTE RAG

# ============================================================

def agente\_beneficios(pregunta):

"""

Recibe una pregunta del usuario, busca información relevante en el Vector Store de Beneficios y Compensaciones y genera una respuesta utilizando Gemini. """

# \--------------------------------------------------------

# Recuperar documentos relevantes

# \--------------------------------------------------------

documentos\_relevantes = retriever\_beneficios.invoke( pregunta )

# \-------------------------------------------------------# Verificar si se encontró información # --------------------------------------------------------

if not documentos\_relevantes:

return "No encontré información suficiente en la base documental proporcionada."

# \--------------------------------------------------------

# Unir el contenido de los documentos recuperados

# \--------------------------------------------------------

contexto = "\\n\\n".join(

documento.page\_content for documento in documentos\_relevantes )

# \--------------------------------------------------------

# Construir el prompt con el contexto recuperado

# \--------------------------------------------------------

prompt\_final = PROMPT\_BENEFICIOS.format( context=contexto, question=pregunta )

# \-------------------------------------------------------# Enviar el prompt al modelo Gemini # --------------------------------------------------------

respuesta = llm\_beneficios.invoke( prompt\_final )

# \--------------------------------------------------------

# Retornar únicamente el contenido de la respuesta

# \--------------------------------------------------------

return respuesta.content

# ============================================================ # 3. VERIFICACIÓN DEL AGENTE

# ============================================================

print("Agente RAG de Beneficios y Compensaciones creado correctamente.")

# **RESULTADO ESPERADO:**

AGENTE RAG DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.

* **Nota metodológica:** Los agentes **7 (Reglamento Interno)** y **8 (Reclutamiento y Onboarding)** se configuran siguiendo **exactamente los mismos pasos operativos (6.1 al 6.5)** , modificando únicamente las rutas de origen documental y los metadatos específicos del dominio.

# **9. Agente de Acción — Registro de solicitudes**

Este componente gestiona las transacciones operativas del sistema registrando solicitudes formales de los colaboradores.

# **9.1. Creación de la herramienta de registro de solicitudes**

**Ejecuta esta celda ( aquí solo muestra una parte del código-en el archivo esta todo el código):** import uuid from datetime import datetime, timedelta from pathlib import Path

# ============================================================ #   HERRAMIENTA DE ACCIÓN (REGISTRO DE SOLICITUDES)

# ============================================================

def registrar\_solicitud( tipo\_solicitud: str, colaborador: str = "N/A", detalle: str = "N/A", fecha\_inicio: str = "N/A", fecha\_fin: str = "N/A", dias: str = "N/A", jefe: str = "N/A", nombre\_dependiente: str = "N/A", vinculo: str = "N/A", documentos\_respaldo: str = "N/A" ) -> str:

tipo = tipo\_solicitud.strip().lower()

print( "Herramienta de acción " "(registro de solicitudes) creada correctamente." ) print("Ruta del archivo de registro:") print(REGISTRO\_PATH)

# **RESULTADO ESPERADO:**

Herramienta de acción (registro de solicitudes) creada correctamente. Ruta del archivo de registro: C:\\Users\\USER\\MI PROYECTO\\registro\_solicitudes\_rrhh.txt

# **10. Orquestador Central**

El orquestador actúa como el núcleo de direccionamiento del sistema, evaluando la intención del usuario y delegando la ejecución al agente especializado correspondiente.

# **10.1. Creación y configuración del Orquestador Central**

# **Ejecuta esta celda ( aquí solo muestra una parte del código-en el archivo esta todo el código):**

import uuid from langchain.agents import create\_agent from langgraph.checkpoint.memory import InMemorySaver from langchain.tools import tool

# ============================================================

# EL ORQUESTADOR CENTRAL DE RECURSOS HUMANOS

# ============================================================

# ============================================================ # 1. DEFINICIÓN DE LAS TOOLS

# ============================================================

# @tool

def consultar\_beneficios(pregunta: str) -> str: """ Responde preguntas sobre beneficios, compensaciones, seguros médicos, bonos y otros beneficios corporativos. """ return agente\_beneficios(pregunta)

@tool def consultar\_reglamento(pregunta: str) -> str: """ Responde preguntas sobre el Reglamento Interno, jornada laboral, vacaciones, permisos, código de conducta y faltas y sanciones. """ return agente\_reglamento(pregunta)

@tool

def consultar\_reclutamiento(pregunta: str) -> str: """ Responde preguntas sobre procesos de selección, vacantes, programa de referidos e inducción (onboarding). """ return agente\_reclutamiento(pregunta) **Resultado esperado:**

Orquestador de Recursos Humanos creado exitosamente con las siguientes tools:

* consultar\_beneficios
* consultar\_reglamento
* consultar\_reclutamiento
* registrar\_solicitud\_rrhh

# **11. Mini App**

**Ejecuta esta celda (aquí solo muestra una parte del código debido a la extensión-en el archivo esta todo el código):**

import ipywidgets as widgets

import html as html\_lib

import re

import uuid

from datetime import datetime

from pathlib import Path

from langchain.agents import create\_agent

from langgraph.checkpoint.memory import InMemorySaver

from langchain.tools import tool

# **RESULTADO ESPERADO:**

**RUTA PRINCIPAL DEL PROYECTO: C:\\USERS\\USER\\MI PROYECTO**

# **ARCHIVO DE REGISTRO VINCULADO: TRUE**

