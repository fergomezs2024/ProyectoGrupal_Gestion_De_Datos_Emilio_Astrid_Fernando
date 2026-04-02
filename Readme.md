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
