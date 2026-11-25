## TP Final Seminario de Actualización 2do cuatrimestre 2026
## Guía de conexión: Backend (Render) ↔ Frontend (GitHub Pages)

El frontend y el backend van a estar en dominios distintos, por lo que el navegador va a
bloquear las peticiones salvo que el backend **permita explícitamente** el origen del
frontend. Eso se resuelve con **CORS** en FastAPI. Esta guía indica los pasos a seguir.

---

## 1. Archivos del frontend (carpeta `frontend-prediccion-ventas/`)

| Archivo | Función |
|---|---|
| `index.html` | Estructura: formulario con 3 campos (TV, Radio, Prensa), botón de predicción, área de resultado e historial. |
| `style.css` | Estilos responsive, paleta teal, estados de carga/error/éxito. |
| `cliente.js` | **Cliente HTTP.** Toda la configuración de conexión (URL de Render, nombre del endpoint, nombres de campos, timeout) está acá, al principio del archivo. |
| `script.js` | Lógica de la app: validación, sincronización de deslizadores, llamado a la API, renderizado del resultado e historial en `localStorage`. |

---

## 2. Paso a paso para conectarlos

### Paso 1 — Decirle al frontend dónde está el backend

Abrir `cliente.js` y modificar el bloque de configuración al principio:

```js
const API_BASE_URL = "https://TU-APP-EN-RENDER.onrender.com"; // ← la URL de Render, sin barra final
const ENDPOINT_PREDICCION = "/predecir";                       // ← la ruta definida en rutas/prediccion.py
const CAMPOS_SOLICITUD = { tv: "tv", radio: "radio", prensa: "prensa" }; // ← nombres que espera el Pydantic
```

> Si todavía no se desplegó el backend en Render, cumplir los pasos 3–4 para obtener la
> URL, y después volver a completar este paso.

### Paso 2 — Habilitar CORS en el `main.py`

En `backend/main.py`, agregar el middleware de CORS. Es lo único que permite que
GitHub Pages llame a Render.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI(title="Predicción de Ventas")

# Permitir el origen del frontend en GitHub Pages.
# Cuando se tenga la URL definitiva, reemplazar "*" por el origen (ver Paso 6).
origins = [
    "https://TU-USUARIO.github.io",   # ← Sitio de GitHub Pages (sin la ruta del repo)
    "http://localhost:8000",          # Para probar localmente
    "http://127.0.0.1:5500",          # Live Server de VS Code, si se usa
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,      # o ["*"] en pruebas; después restrictivo
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# ... el resto de main.py (inclusión de rutas, etc.)
```

> Mientras `allow_origins=["*"]` funciona para pruebas, en producción colocar únicamente
> el origen de GitHub Pages. Con `allow_credentials=True` no se puede usar `"*"`, así que
> si se deja `True`, la lista de origins debe ser explícita.

### Paso 3 — Preparar el backend para Render

Render necesita saber qué instalar y cómo arrancar. Crear en la raíz de la carpeta
`backend/` (o del repo) un archivo `requirements.txt` con al menos:

```
fastapi
uvicorn[standard]
joblib
scikit-learn
numpy
pandas
```

> **Importante:** la versión de `scikit-learn` en Render debe ser la **misma** con la
> que se entrenó y guardo el `modelo.joblib`, o al menos compatible. Si no, al cargar el
> modelo puede fallar o dar predicciones incorrectas. Fijar la versión exacta, p. ej.
> `scikit-learn==1.5.2`.

Comando de arranque en Render (Start Command):

```
uvicorn main:app --host 0.0.0.0 --port $PORT
```

(Asegurarse que Render pueda encontrar `main.py` y la carpeta `modelo/`; si `main.py`
está dentro de `backend/`, usar `uvicorn backend.main:app ...` o configurar el Root Directory.)

### Paso 4 — Desplegar el backend en Render

1. Subir carpeta `backend` a un repositorio de GitHub (puede ser el mismo o uno distinto al del frontend).
2. En Render → **New +** → **Web Service** → conectar el repo.
3. Runtime: **Python 3**.
4. Build Command: `pip install -r requirements.txt`.
5. Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`.
6. Una vez desplegado, copiar la URL (ejemplo: `https://mi-api.onrender.com`).
7. Verificar abriendo `https://mi-api.onrender.com/docs` en el navegador: debería verse la documentación interactiva de FastAPI. Probar el endpoint de predicción desde ahí.

### Paso 5 — Desplegar el frontend en GitHub Pages

1. Crear un repositorio (ej: `prediccion-ventas-frontend`) y subir los 4 archivos del frontend en la raíz (o en una carpeta, si se utiliza `docs/`).
2. En el repo → **Settings** → **Pages** → Source: **Deploy from a branch** → elegir `main` / carpeta `/ (root)` (o `/docs`).
3. Guardar. En unos minutos el sitio estará en `https://TU-USUARIO.github.io/NOMBRE-REPO/`.

### Paso 6 — Cerrar el círculo de CORS

Con la URL definitiva de GitHub Pages, volver a `main.py` y asegurarse de que ese origen
esté en `allow_origins` (con o sin ruta del repositorio, el origen es solo
`https://TU-USUARIO.github.io`). Volver a desplegar el backend en Render (se hace solo al
pushear a GitHub).

---

## 3. Cómo probar localmente antes de desplegar

1. Backend: `uvicorn main:app --reload` (queda en `http://localhost:8000`).
2. Frontend: abrir `index.html` con Live Server de VS Code (o cualquier servidor estático).
3. En `cliente.js`, colocar temporalmente `API_BASE_URL = "http://localhost:8000"`.
4. Asegurarse que `http://127.0.0.1:5500` (o el puerto que use Live Server) esté en `allow_origins`.
5. Probar una predicción. Cuando funcione, cambiar la URL a la de Render y desplegar.

---

## 4. Problemas comunes y cómo solucionarlos

| Síntoma en el navegador (Consola F12) | Causa | Solución |
|---|---|---|
| `CORS error` / `blocked by CORS policy` | Falta `CORSMiddleware` o el origen no está en `allow_origins`. | Paso 2 y 6. Verificar que el origen incluya `https://` y que no tenga barra final. |
| `422 Unprocessable Entity` | Los nombres de los campos que envía el frontend no coinciden con el modelo Pydantic. | Ajustar `CAMPOS_SOLICITUD` en `cliente.js` para que coincidan con los nombres de `BaseModel`. |
| `Mixed Content` / `blocked:mixed-content` | El frontend (HTTPS) llama a un backend HTTP. | Usar siempre `https://` en la URL de Render (Render lo provee gratis). |
| Primera petición tarda ~30 s y luego es rápida | Plan gratuito de Render se suspende por inactividad. | Normal. La interfaz ya muestra un mensaje explicativo. Se puede evitar con un ping periódico o un plan de pago. |
| Error al cargar `modelo.joblib` en Render | Versión distinta de scikit-learn, o ruta del archivo incorrecta. | Fijar versiones en `requirements.txt` y usar rutas relativas con `pathlib` (`Path(__file__).parent / "modelo" / "modelo.joblib"`). |
| `Failed to fetch` | URL mal escrita, backend caído, o firewall. | Verificar la URL en `cliente.js` y que `/docs` abra en el navegador. |

---

## 5. Contrato de API esperado por el frontend

El frontend envía un `POST` a `{API_BASE_URL}{ENDPOINT_PREDICCION}` con cuerpo:

```json
{ "tv": 150, "radio": 30, "prensa": 40 }
```

Y espera una respuesta JSON con la predicción. El cliente la busca automáticamente en
cualquiera de estas claves (en orden): `prediccion`, `prediccion_ingresos`,
`prediccion_ventas`, `ingresos`, `ventas`, `valor`, `resultado`, `prediction`, `sales`
(también dentro de objetos anidados `data`/`detalle`/`resultado`). Si la API devuelve
otra clave, ajustar `_extraerPrediccion` en `cliente.js`.

Ejemplo de respuesta válida:

```json
{ "prediccion": 17.35 }
```
