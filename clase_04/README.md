---
title: Clase 04 Seminario Actualizacion 2026
emoji: 🐨
colorFrom: purple
colorTo: red
sdk: gradio
sdk_version: 6.32.0
python_version: '3.12'
app_file: app.py
pinned: false
license: mit
short_description: Clase 4 Seminario Actualizacion 2026
---

## Clase N° 4 - Del repositorio local al Spaces

#### 🎯 Objetivos:
Dirección web pública activa. Creación de cuenta en Hugging Face. Respositorio local conectado en dos lugares distintos.

#### 📂 Contenido de esta Carpeta
| Nombre del archivo | Descripción |
|---|---|
| `app.py` | Contiene el código de una pequeña app en Gradio|
| `README.md` | Pasos para editar archivo `app.py` |
| `requirements.txt` | Archivo de texto con las librerías necesarias para el entorno virtual |

#### ⚙️ Requisitos Previos
Antes de ejecutar el código, asegurarse de tener instalado:
- Visual Studio Code (o cualquier editor de código de preferencia)
- Python 3.8 o superior
- Cuenta en GitHub con repositorios enlazados.

#### 💻 Instalación y Uso con Git

```bash
# Clonar repositorio
git clone https://github.com/sebsmedina/seminario_actualizacion_2026/clase_03
cd clase_03     # en repositorio local
```

#### ✅ Pasos

1. Generar un usuario y una cuenta en https://huggingface.co/

Hugging Face brinda **Spaces**, una herramienta gratuita que permite alojar, probar y compartir aplicaciones web interactivas de inteligencia artificial y aprendizaje automático.

2. En el perfil de usuario, ir a la sección de **Access Tokens** y crear un nuevo token con permisos de escritura ("write").

3. Crear un Space en HF para poder alojar allí el repositorio. La diferencia entre GitHub y HF es que éste último levanta el código y lo deja funcionando.

4. Una vez generado el Space, se observa que posee su primer commit y un README.md creado automáticamente:

```bash
---
title: Mi App
emoji: 🚀
colorFrom: blue
colorTo: green
sdk: gradio
sdk_version: ...
app_file: app.py
pinned: false
---

```
Ésto indica que el sitio del repositorio en HF funciona correctamente. Copiar la URL.

5. En la carpeta `clase_04` en el repositorio local, generar un nuevo destino remoto con el link del Space. Al enlazar, solicitará autorizar la conexión con el sitio de HF mediante el token de escritura (WRITE) generado anteriormente:

```bash
git remote add space https://huggingface.co/spaces/USUARIO/NOMBRE
git remote -v

```

"space" es el alias del nuevo repositorio remoto, en HF.

6. Ejecutar un push a dicho repo:

```bash
git push space main
```

Observar que devuelve un error. Ésto es esperable debido a la coexistencia del `README.md` generado de manera local (o al clonar ésta carpeta) y el creado en el Space de HF. 
Con la siguiente linea se declara a Git que tiene permiso para mezclarlos:

```bash
git pull space main --allow-unrelated-histories
```

Git sugiere un nuevo README.md con la siguiente estructura:

```bash
<<<<<<< HEAD
(tu README, el de la Clase 3)
=======
(el README del Space, con su configuración)
>>>>>>>
```

7. Seleccionar la versión inicial (`clase_03`),eliminar la corrección y los marcadores. Ejecutar *add*, *commit* y *push*:

```bash
git add README.md
git commit -m "Resuelvo el conflicto del README"
git push space main
```

El Space se construye, pero al ir al espacio inicial devuelve un nuevo error.

8. El README.md creado por el Space contiene la configuración de la plataforma. Sin ella, no puede funcionar. Retroceder en los cambios, preservar la sugerencia (albergando ambos README.md en el mismo archivo) y ejecutar nuevamente *add*, *commit* y *push*:

```bash
# Repone el encabezado al inicio del README
git add README.md
git commit -m "Repongo la configuración del Space"
git push space main
```

La aplicación se deplega de forma correcta, siendo Gradio quien construye el frontend, construye la API y la documenta.

9. Para finalizar, modificar la interfaz de la app utilizando el método Blocks de Gradio, agregándole funcionalidad.
