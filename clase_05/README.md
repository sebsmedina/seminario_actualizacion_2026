## Clase N° 5 - Del link que se cae al deploy propio

#### 🎯 Objetivos:
Deploy de apps en Render con diferentes interfaces.

#### 📂 Contenido de esta Carpeta
```text
└── Clase_05/
       ├── gradio/
       │       ├── app.py
       │       ├── requirements.txt
       │       └── README.md  
       └── streamlit/
               ├── app.py
               ├── requirements.txt
               └── README.md
```
#### ⚙️ Requisitos Previos
- Poseer el código de la aplicación para ser desplagada tanto por Render (con la interfaz en Gradio) como por Streamlit.
- Separar los contenidos en carpetas distintas para ambos despliegues.

#### 🚀 Links:
- Gradio en Render: https://seminario-actualizacion-2026-p6k5.onrender.com/
- Streamlit en Render: https://seminario-actualizacion-2026-clase-05.onrender.com

#### ✅ ¿Qué hacen mis aplicaciones?
Simplemente a partir del ingreso por parte del usuario de la fecha de nacimiento, ambas calculan la edad del mismo.

#### 🧠 ¿Cómo se publicaron ambas?
Para desplegar ambas publicaciones se utilizó Render, un web service gratuito.

- Gradio App
Root Directory: clase_05/gradio/
Build Command: clase_05/gradio/ pip install -r requirements.txt
Start Command: clase_05/gradio/ python app.py

- Streamlit App
Root Directory: clase_05/streamlit/
Build Command: clase_05/streamlit/ pip install -r requirements.txt
Start Command: clase_05/streamlit/  streamlit run app.py --server.port $PORT -- server.address 0.0.0.0 --server.headless true

#### 📋 Desarrollo

1.  Generar una cuenta en Render para poder deployar nuestra app. Ventajas: gratuito, soporta ASGI y se conecta desde GitHub.

2. Eliminar las siguientes líneas en el código de la app que hacían referencia al decorador de Spaces de Hugging Face (agregadas en la clase 02) ya que no deployaremos por esa vía:

```bash
import spaces
@spaces.GPU
```
3. Agregar las siguientes que configuran el puerto de forma correcta para Render (localhost solo es accesible desde la máquina misma, no puede atender solicitudes del exterior):

```bash
import os
demo.launch(
    server_name="0.0.0.0",
    server_port=int(os.environ.get("PORT", 7860))
)
```
Actualizar el repositorio luego de los cambios con *add*, *commit* y *push*.

4. En el hub de Render ir al boton *+New* y seleccior *New Web Service*. Allí seleccionar el repositorio que aloja la app para Gradio (`clase_05\gradio`). Completar el apartado *Start Command* con:

```bash
python app.py
```

En *Compute* pinchar en la opción *Free* y luego en el botón *Deploy Web Service* y listo! La app está en proceso de ser desplegada.

5. App en **Streamlit**.
En el hub de Render ir al boton *+New* y seleccior *New Web Service*. Allí seleccionar el repositorio que aloja la app para Streamlit(`clase_05\streamlit`). Completar el apartado *Start Command* con:

```bash
streamlit run app.py --server.port $PORT --server.address 0.0.0.0 --server.headless true
```

#### 🧠 Conclusiones - Diferencias entre ambas

Gradio se codifica menos y es mas directo con su método *.Blocks()*, pero más rigido, mientras que en Streamlit es más personalizable, pero con mayor curva de aprendizaje. Por otro lado, Gradio te ayuda a compartir rápido con una URL pública (con el método demo.launch(share=True)), mientras que en Streamlit hay que subir el código a un repositorio y conectar con el hosting.

En resumen, por su características Gradio está orientado a los modelos de ML e IA, mientras que Streamlit lo es para dashboards y aplicaciones de datos.