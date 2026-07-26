README - Sistema Inteligente de Mesa de Ayuda de Recursos Humanos (Patito S.A.)
1. Introducción
El presente proyecto implementa un prototipo inteligente de mesa de ayuda para Recursos Humanos en Patito S.A. utilizando LangChain, Google Gemini y tecnología RAG (Retrieval-Augmented Generation). El sistema está diseñado para actuar como un asistente corporativo estructurado, capaz de coordinar múltiples agentes especializados para resolver consultas normativas y registrar solicitudes operativas de manera automatizada.
2. Instalación de Librerías
Para configurar el entorno de ejecución, ejecute la celda correspondiente para instalar las dependencias principales que soportan la arquitectura de LangChain, la integración con Google Gemini y la persistencia vectorial:
%pip install -U langchain langchain-google-genai langchain-community langchain-chroma chromadb python-dotenv ipywidgets
Resultado esperado:
[NOTICE] A NEW RELEASE OF PIP IS AVAILABLE: 24.0 -> 26.1.2
[NOTICE] TO UPDATE, RUN: PYTHON.EXE -M PIP INSTALL --UPGRADE PIP
3. Importación de Librerías
Ejecute la celda de importación para cargar las librerías necesarias de manejo de rutas, variables de entorno, modelos de lenguaje de Google Gemini, cargadores de texto, separadores y bases vectoriales Chroma.
Resultado esperado:
LIBRERÍAS IMPORTADAS CORRECTAMENTE.
4. Configuración de la API de Google Gemini
Para permitir la comunicación segura con los modelos de lenguaje, es necesario configurar la clave de acceso (GOOGLE_API_KEY) utilizando un archivo de entorno .env.
4.1 — Creación del archivo .env
Ejecute la celda para definir la ruta del proyecto, solicitar de forma segura la clave mediante getpass y guardarla en el archivo de entorno correspondiente.
Resultado esperado:
INGRESA TU GOOGLE_API_KEY: [ENTRADA OCULTA] 
¡LISTO! ARCHIVO .ENV CREADO CORRECTAMENTE EN: /HOME/USER/MI PROYECTO/.ENV
4.2 — Verificación y carga de la clave
Ejecute la celda para verificar y cargar formalmente la clave en el entorno de ejecución del sistema.
Resultado esperado:
GOOGLE_API_KEY CONFIGURADA CORRECTAMENTE.
5. Configuración de Modelos de IA
Ejecute la celda de inicialización para definir las variables globales de los modelos y comprobar el correcto funcionamiento del objeto LLM.
Resultado esperado:
MODELO LLM: GEMINI-3.1-FLASH-LITE
MODELO DE EMBEDDINGS: GEMINI-EMBEDDING-2-PREVIEW
OBJETO LLM INICIALIZADO CORRECTAMENTE.
5.1 Configuración central de rutas
Ejecute la celda que consolida todas las rutas absolutas del proyecto, directorios de documentos, bases vectoriales y el archivo de registro de solicitudes.
Resultado esperado:
Ruta principal del proyecto:
C:\Users\USER\MI PROYECTO

Documentos:
Beneficios: C:\Users\USER\MI PROYECTO\Documentos\01_Beneficios_Compensaciones.txt -> True
Reglamento: C:\Users\USER\MI PROYECTO\Documentos\02_Reglamento_Interno.txt -> True
Reclutamiento: C:\Users\USER\MI PROYECTO\Documentos\03_Reclutamiento_Onboarding.txt -> True

Archivo de solicitudes:
Registro: C:\Users\USER\MI PROYECTO\registro_solicitudes_rrhh.txt -> True
Observación: Si en “Registro” la primera ejecución arroja FALSE, se debe a que el archivo físico aún no ha sido creado; este aparecerá automáticamente al realizar el primer registro de solicitud.
6. Agente RAG — Beneficios y Compensaciones
Este módulo procesa la base de conocimiento documental para habilitar respuestas precisas fundamentadas en el contexto corporativo.
6.1 Carga del documento
Ejecute la celda para realizar la lectura inicial del archivo de texto mediante TextLoader.
Resultado esperado:
DOCUMENTO DE BENEFICIOS Y COMPENSACIONES CARGADO CORRECTAMENTE.
6.2 Chunking
Segmentación estructurada del texto en bloques manejables mediante un divisor de texto recursivo y expresiones regulares.
Resultado esperado:
Total de chunks: 4
Tamaño promedio: 280 caracteres
Tamaño min/max: 113 / 749 caracteres

Inicio de cada chunk:
------------------------------------------------------------
  ch_00: 1. SEGURO MÉDICO CORPORATIVO 1.1 Cobertura: consultas m...
  ch_01: 2. BONOS - Bono por desempeño anual según evaluación. -...
  ch_02: 3. OTROS BENEFICIOS - Día libre de cumpleaños. - Capaci...
  ch_03: 4. COMPENSACIÓN La estructura salarial considera el rol...
6.3 Embeddings + Chroma
Transformación vectorial de los fragmentos y almacenamiento persistente en ChromaDB.
Resultado esperado:
VECTOR STORE DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.

Ruta del Vector Store:
C:\USERS\USER\MI PROYECTO\VECTORSTORES\BENEFICIOS_COMPENSACIONES

Contenido generado por Chroma:
- BB6E7CD1-D785-46EB-8477-9B8DA83C1BC4
- CHROMA.SQLITE3
6.4 Retriever
Configuración del recuperador de similitud para extraer los fragmentos más relevantes ante una consulta.
Resultado esperado:
RETRIEVER DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.
Cantidad de documentos recuperados por consulta: 5
6.5 Creación del Prompt del Agente RAG de Beneficios y Compensaciones
Definición de directrices estrictas para limitar las respuestas al contexto provisto y evitar alucinaciones.
Resultado esperado:
PROMPT DEL AGENTE DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.
El prompt incluye las reglas de uso exclusivo de la base documental.
6.6 Creación del Agente RAG
Instanciación formal del agente consultor que une la recuperación y la generación con Google Gemini.
Resultado esperado:
AGENTE RAG DE BENEFICIOS Y COMPENSACIONES CREADO CORRECTAMENTE.
Nota metodológica: Los agentes 7 (Reglamento Interno) y 8 (Reclutamiento y Onboarding) se configuran siguiendo exactamente los mismos pasos operativos (6.1 al 6.5), modificando únicamente las rutas de origen documental y los metadatos específicos del dominio.
9. Agente de Acción — Registro de solicitudes
Este componente gestiona las transacciones operativas del sistema registrando solicitudes formales de los colaboradores.
9.1. Creación de la herramienta de registro de solicitudes
Ejecute la celda para inicializar la herramienta de acción con sus respectivos parámetros y validaciones.
Resultado esperado:
Herramienta de acción (registro de solicitudes) creada correctamente.
Ruta del archivo de registro:
C:\Users\USER\MI PROYECTO\registro_solicitudes_rrhh.txt
10. Orquestador Central
El orquestador actúa como el núcleo de direccionamiento del sistema, evaluando la intención del usuario y delegando la ejecución al agente especializado correspondiente.
10.1. Creación y configuración del Orquestador Central
Ejecute la celda de definición de herramientas (@tools) y el sistema de ruteo.
Resultado esperado:
Orquestador de Recursos Humanos creado exitosamente con las siguientes tools:
  - consultar_beneficios
  - consultar_reglamento
  - consultar_reclutamiento
  - registrar_solicitud_rrhh
11. Mini App
Ejecute la celda de la interfaz gráfica e integración con el orquestador para iniciar las pruebas finales del sistema conversacional de Recursos Humanos.
Resultado esperado:
RUTA PRINCIPAL DEL PROYECTO: C:\USERS\USER\MI PROYECTO
ARCHIVO DE REGISTRO VINCULADO: TRUE
