# SMART MIRROR PARIS

Espejo Inteligente para Probadores de Tienda Retail

Documento 6: README.md  |  Actividades 1.3.2 y 1.4.2  |  ITY1101 - Gestión de Datos para IA

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python)]()
[![FastAPI](https://img.shields.io/badge/FastAPI-latest-009688?logo=fastapi)]()
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)]()
[![Electron](https://img.shields.io/badge/Electron-28-47848F?logo=electron)]()
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql)]()
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)]()
[![MediaPipe](https://img.shields.io/badge/MediaPipe-Edge_AI-FF6F00?logo=google)]()
[![YOLO](https://img.shields.io/badge/YOLO-ONNX_Runtime-FF4C00)]()
[![Git Flow](https://img.shields.io/badge/Git_Flow-v1-F05032?logo=git)]()
[![License](https://img.shields.io/badge/License-MIT-28A745)]()

## 1. Descripción del Proyecto y Valor que Entrega

Smart Mirror Paris es un sistema de probador virtual que transforma la experiencia de compra en tiendas retail. Combina Edge AI, visión computacional en tiempo real y consulta de inventario para permitir que un cliente vea cómo le quedaría una prenda antes de tomársela del perchero.

**Propuesta de valor:**
* El cliente se para frente al espejo: sin esperar turno de probador, sin desvestirse.
* El sistema detecta su pose en tiempo real mediante Edge AI (MediaPipe + YOLO corriendo localmente, sin enviar video a la nube).
* El cliente toca la prenda en pantalla y la ve superpuesta sobre su imagen en vivo.
* El espejo muestra en tiempo real el stock disponible en su talla y color seleccionados.

**Problema que resuelve:** los probadores físicos generan esperas de 8-12 minutos en temporada alta y una tasa de devolución del 30-40% por talla incorrecta. Smart Mirror Paris elimina ambos cuellos de botella con tecnología que corre íntegramente en el hardware del local.

## 2. Stack Tecnológico

# README.md — Smart Mirror Paris

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python)]()
[![FastAPI](https://img.shields.io/badge/FastAPI-latest-009688?logo=fastapi)]()
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)]()
[![Electron](https://img.shields.io/badge/Electron-28-47848F?logo=electron)]()
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql)]()
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)]()
[![MediaPipe](https://img.shields.io/badge/MediaPipe-Edge_AI-FF6F00?logo=google)]()
[![YOLO](https://img.shields.io/badge/YOLO-ONNX_Runtime-FF4C00)]()
[![Git Flow](https://img.shields.io/badge/Git_Flow-v1-F05032?logo=git)]()
[![License](https://img.shields.io/badge/License-MIT-28A745)]()

## 3. Árbol de Carpetas del Repositorio

```bash
smart-mirror-paris/
|-- docker-compose.yml          # Orquestacion completa del sistema
|-- .env.example                # Variables de entorno de ejemplo
|-- .gitignore                  # Excluye .env, __pycache__, node_modules, *.onnx
|-- README.md                   # Este archivo
|
|-- backend/                    # Modulo FastAPI
|   |-- app/
|   |   |-- main.py             # Entry point FastAPI
|   |   |-- routers/            # productos.py, stock.py, sesion.py, overlay.py
|   |   |-- models/             # Modelos SQLAlchemy
|   |   |-- schemas/            # Schemas Pydantic
|   |   `-- database.py         # Configuracion de conexion BD
|   |-- migrations/             # Scripts Alembic
|   |-- tests/                  # pytest: test_*.py
|   |-- Dockerfile
|   `-- requirements.txt
|
|-- ia_engine/                  # Motor IA (Edge AI)
|   |-- main.py                 # Entry point + WebSocket server
|   |-- pose/detector.py        # Wrapper MediaPipe
|   |-- segmentation/yolo.py    # Inferencia YOLO-ONNX
|   |-- overlay/blender.py      # Superposicion de textura
|   |-- models/                 # *.onnx (no commiteados, descargados en build)
|   |-- pipeline/
|   |   |-- ingest.py           # Ingesta de imagenes
|   |   |-- clean.py            # Limpieza y normalizacion
|   |   |-- transform.py        # Anotacion y transformacion
|   |   `-- validate.py         # Validacion de calidad
|   |-- Dockerfile
|   `-- requirements.txt
|
|-- frontend/                   # React + Electron
|   |-- src/
|   |   |-- components/         # CameraFeed, GarmentSelector, OverlayCanvas, StockBadge
|   |   |-- services/           # api.ts, ws.ts, types.ts
|   |   `-- pages/              # MirrorView.tsx, GarmentList.tsx
|   |-- electron/main.js        # Entry point Electron
|   `-- package.json
|
|-- database/
|   |-- init.sql                # DDL: CREATE TABLE ...
|   `-- seed.sql                # Datos de prueba
|
`-- docs/
    |-- diagramas/              # Archivos *.mmd (Mermaid)
    `-- documentos/             # Docs 1-6 en PDF/DOCX
```

## 4. Instrucciones de Instalación (Copy/Paste)

Requisitos previos: Git, Docker Desktop 24+, Node.js 20+, Python 3.11+. El sistema completo levanta con Docker Compose en un solo comando.

### Paso 1 — Clonar el repositorio
```bash
git clone https://github.com/tu-usuario/smart-mirror-paris.git
cd smart-mirror-paris
```

### Paso 2 — Configurar variables de entorno
```bash
cp .env.example .env
# Editar .env con tus valores (editor de preferencia):
# POSTGRES_USER=mirror_user
# POSTGRES_PASSWORD=tu_password_seguro
# POSTGRES_DB=smart_mirror
# BACKEND_PORT=8000
# IA_ENGINE_PORT=8001
```

### Paso 3 — Levantar Docker Compose (BD + Backend)
```bash
# Construye imagenes y levanta todos los servicios
docker compose up --build -d

# Verificar que todos los contenedores esten corriendo:
docker compose ps

# Esperar ~30 segundos y verificar la API:
curl http://localhost:8000/docs
# Debe mostrar la interfaz Swagger de FastAPI
```

### Paso 4 — Instalar dependencias Python del Motor IA (desarrollo local)
```bash
cd ia_engine
python -m venv venv

# En Linux/macOS:
source venv/bin/activate
# En Windows:
venv\Scripts\activate

pip install -r requirements.txt

# Verificar instalacion de MediaPipe y ONNX:
python -c "import mediapipe; import onnxruntime; print('OK')"
```

### Paso 5 — Ejecutar el pipeline de IA (primera vez)
```bash
# Desde ia_engine/ con el venv activo:

# 1. Ingestar imagenes de prendas
python pipeline/ingest.py --source ./data/raw --output ./data/ingested

# 2. Limpiar y normalizar
python pipeline/clean.py --input ./data/ingested --output ./data/clean

# 3. Transformar y anotar
python pipeline/transform.py --input ./data/clean --output ./data/annotated

# 4. Validar calidad del dataset
python pipeline/validate.py --input ./data/annotated
# Output esperado: 'Tasa de rechazo: X.X% (< 5% OK)'

# 5. Iniciar el motor IA (WebSocket server en puerto 8001)
python main.py
# Output esperado: 'IA Engine running on ws://localhost:8001'
```

### Paso 6 — Levantar el Frontend (React + Electron)
```bash
cd ../frontend

# Instalar dependencias Node.js
npm install

# Modo desarrollo (React en browser):
npm run dev
# Abre http://localhost:3000 en el navegador

# Modo produccion (empaquetado Electron):
npm run electron:build
# Genera ejecutable en frontend/dist/

# Ejecutar Electron directamente (sin build):
npm run electron:start
```

## 5. Flujo de Funcionamiento Básico

Secuencia completa desde que el cliente se para frente al espejo hasta que decide comprar:

| # | Acción | Detalle técnico |
|---|---|---|
| 1 | Cliente se para frente al espejo | La cámara USB captura frames a 60fps. El Motor IA (ia_engine/main.py) inicia el stream y comienza la detección de pose con MediaPipe. El Frontend muestra la imagen de la cámara con los puntos de pose superpuestos. |
| 2 | Frontend muestra catálogo de prendas | El componente GarmentSelector consulta GET /productos al Backend (FastAPI). La BD retorna el listado de prendas activas con imagen y precio. Se muestra en la pantalla lateral del espejo. |
| 3 | Cliente toca una prenda en pantalla | El Frontend envía un mensaje por WebSocket al Motor IA: `{action: 'select_garment', prenda_id: 'uuid-...', texture_url: '/assets/textura.png'}`. Simultáneamente consulta GET /stock/{prenda_id}?talla=M al Backend. |
| 4 | Motor IA aplica overlay en tiempo real | Por cada frame (~30fps): YOLO-ONNX segmenta la región del torso/cuerpo. El algoritmo blender.py superpone la textura de la prenda sobre la región segmentada. El frame resultante se emite por WebSocket al Frontend. |
| 5 | Frontend renderiza overlay + badge de stock | El componente OverlayCanvas renderiza los frames del WebSocket sobre el video de cámara. El componente StockBadge muestra la disponibilidad recibida del Backend: 'Disponible: 3 unidades' o 'Sin stock en talla M'. |
| 6 | Cliente cambia de prenda o talla | El Frontend envía nuevo mensaje WebSocket con el prenda_id de la nueva selección. El Motor IA cambia la textura activa en el siguiente ciclo de procesamiento. La latencia total (cámara -> overlay visible) es < 300ms. |
| 7 | Cliente decide y se dirige a caja | Al alejarse del espejo, el Motor IA detecta pérdida de pose y cierra la sesión. El Backend registra en tabla SESION: duración, prendas vistas, talla_estimada. Sin datos biométricos almacenados. |

### Endpoints disponibles para prueba:
* **GET** `http://localhost:8000/docs` # Swagger UI - documentacion interactiva
* **GET** `http://localhost:8000/productos` # Lista completa de prendas activas
* **GET** `http://localhost:8000/stock/{id}` # Stock de una variante especifica
* **POST** `http://localhost:8000/sesion` # Crear nueva sesion
* **PUT** `http://localhost:8000/sesion/{id}` # Cerrar sesion con estadisticas
* **WS** `ws://localhost:8001` # WebSocket del Motor IA (frames procesados)

### Verificacion rapida del sistema completo:
```bash
# Desde la raiz del proyecto, con todos los servicios corriendo:

# 1. Verificar backend
curl http://localhost:8000/productos | python -m json.tool

# 2. Verificar base de datos
docker exec smart_mirror_db psql -U mirror_user -d smart_mirror -c 'SELECT count(*) FROM producto;'

# 3. Ejecutar tests del backend
docker exec smart_mirror_backend pytest tests/ -v --cov=app --cov-report=term
# Objetivo: cobertura >= 80%

# 4. Ver logs del motor IA
docker compose logs ia_engine -f
# Debe mostrar: 'IA Engine running' y 'MediaPipe model loaded'
```
