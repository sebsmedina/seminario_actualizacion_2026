## Clase N° 2 - Preparación del entorno de desarrollo

#### 🎯 Objetivos:
Carpeta del Proyecto Integrador. Entorno virtual creado y activado. *requirements.txt* generado. Git configurado. Repositorio local con nuestro primer commit.

#### 📂 Contenido de esta Carpeta
| Nombre del archivo | Descripción |
|---|---|
| `app.py` | Contiene el código de una pequeña app en Gradio|
| `README.md` | Pasos para editar nuestro archivo `app.py` |
| `requirements.txt` | Archivo de texto con las librerías necesarias para nuestro entorno virtual |

#### ⚙️ Requisitos Previos
Antes de ejecutar el código, asegúrate de tener instalado:
- Visual Studio Code (o cualquier editor de código de tu preferencia)
- Python 3.8 o superior

#### 💻 Instalación y Uso Local

```bash
# Clonar repositorio
git clone https://github.com/sebsmedina/seminario_2026_proyecto_integrador/clase_02
cd clase_02
```

#### 📋 Desarrollo

1. Dentro de la carpeta que alojará todas las clases, crear una carpeta de nombre `clase_02`. Dentro de ella, y mediante editor de código, creamos un archivo *app.py*, el cual se alojará todo el Back-end de la aplicación.

2. Crear el entorno virtual del proyecto ingresando por la terminal el siguiente comando:

```bash
# crear y activar (Windows)
> python -m venv .venv
> .venv\Scripts\activate

# macOS / Linux
$ python3 -m venv .venv
$ source .venv/bin/activat
```

Cómo verificar si está activado correctamente:

```bash
python -c "import sys;
print(sys.executable)"
```

**CONSEJO**: siempre crear un entorno virtual para cada proyecto. Asegura aislamiento, organización y reproducibilidad.

3. Se utilizará Gradio, una librería de Python que permite crear interfaces web interactivas para modelos de aprendizaje automático y funciones de inteligencia artificial, para gestionar la API y la interfaz de la aplicación.

Instalar todas las librerías necesarias con *pip* utilizando *requierements.txt*

```bash
(.venv) > pip install -r requirements.txt
```
Instalará entre otras librerías:

```bash
gradio        # Framework para desplegar apps basadas en modelos de ML
spaces        # paquete de utilidades de Hugging Face para desplegar trabajos en HF Spaces
streamlit     # otro framework de código abierto
requests      # instala dependencias para consumir API's
```

4. Almacenar en un archivo *requimerents.txt* las librerías instaladas para que puedan ser replicadas en el futuro.

```bash
(.venv) > pip freeze > requirements.txt     # guarda la lista de dependencias
                                            # para replicarla
```

4. Uso de Git para organizar las distintas versiones de nuestro proyecto.

Identificarse con Git al iniciarlo (necesario para ejecutar commits) una única vez.

```bash
> git --version         # Versión de Git

> git config --global user.name "Nombre Apellido"   # Nombre de usuario
> git config --global user.email "tu@correo.com"    # Correo asociado a la cuenta de GitHub
> git config --global init.defaultBranch main       # Setear rama principal
```

5. Creación de *.gitignore* para evitar que Git mueva archivos y/o carpetas que no deseados.
Por ejemplo:
```bash
.venv/          # carpeta del entorno virtual
__pycache__/    # contiene código fuente de Python escrito en bajo nivel
```

6. Para visualizar que el contenido del repositorio local:

```bash
> git init              # Inicia un nuevo repositorio
> git status -u         # Lo que Git ve en la carpeta del proyecto
```

7. Añadir los archivos con las últimas modificaciones al área de preparación (*add*) y almacenarlas en el historial del repositorio local (*commit*).

```bash
> git add .                         # Agrega todos los
                                    # archivos al área de preparación (en éste caso app.py)
> git commit -m "mi_primer_commit"  # Guarda la actualización
                                    # junto con un mensaje descriptivo
```