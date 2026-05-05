# Habits Tracker

App personal mínima de tracking diario de hábitos. HTML + JS puro, sin backend. Datos en localStorage del navegador.

## Uso

1. Abrir `index.html` en navegador.
2. Click en celda → marcar como hecho.
3. Input rápido arriba: escribir nombres parciales, números o "todos" para marcar varios.
4. Cambiar semana con flechas. Botón "Exportar CSV" para histórico.

## Datos

- localStorage key: `habitos_v1`
- Estructura: `{ "YYYY-MM-DD": { "<idx>": true, ... } }`
- Sin servidor. Cero telemetría.

## Notificación diaria (opcional)

Cron via GitHub Actions en `.github/workflows/daily.yml`. Llama a CallMeBot WhatsApp API con secrets `PHONE` y `APIKEY`.

## Stack

- Single-file HTML (mobile-first, light theme)
- Web Manifest para instalar como PWA
- Sin dependencias externas
