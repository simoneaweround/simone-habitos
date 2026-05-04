# Hábitos — Simone

App personal para tracking diario de los 27 hábitos de la tabla.

## Cómo usar la app

1. **Abrir** `index.html` doble click (Mac) → se abre en navegador.
2. **Marcar** un hábito → click en celda. Se pone verde con check. Otro click lo destacha.
3. **Cambiar semana** → flechas ‹ › arriba. "hoy" vuelve a la semana actual.
4. **Exportar CSV** → botón abajo. Descarga histórico completo.
5. **Reset semana** → borra checks de la semana visible.

Los datos se guardan en **localStorage** del navegador (no servidor). Si cambias de máquina, exporta CSV antes.

## Acceso desde celular (recomendado)

Para usar desde iPhone/Android:

**Opción A — instalar como PWA en celular (mejor):**
1. Servir la app desde tu Mac (un comando):
   ```bash
   cd "/Users/edo/Documents/Claude/Archivo Personal Simone/habitos-app"
   python3 -m http.server 8765
   ```
2. En tu celular conectado a la misma red Wi-Fi: abre `http://<IP-de-tu-Mac>:8765` en Safari/Chrome
3. Safari → "Compartir" → "Añadir a inicio" → queda como app nativa

**Opción B — hostear gratis:**
- Subirla a Cloudflare Pages / Netlify / Vercel (free tier). Te paso script de deploy si quieres.

## Notificación diaria por WhatsApp (CallMeBot — gratis)

CallMeBot manda WhatsApp a tu propio número con un GET request. Cero costo, cero infra.

### Setup (5 min)

1. **Activar el bot:** desde tu WhatsApp envía `I allow callmebot to send me messages` al número **+34 644 51 95 23**
2. Te responde con tu **API key personal**. Guárdalo.
3. **Probar:** abre en navegador
   ```
   https://api.callmebot.com/whatsapp.php?phone=521<TU_NUMERO>&text=Marcaste+tus+habitos+hoy?&apikey=<TU_APIKEY>
   ```
   (México es +52, número de 10 dígitos. Ej: 5219999123456)
4. Te llega WhatsApp.

### Cron diario (GitHub Actions — corre aunque la Mac esté apagada)

Repo nuevo en GitHub `simone-habitos-cron` con este `.github/workflows/daily.yml`:

```yaml
name: daily-habits-reminder
on:
  schedule:
    - cron: "30 3 * * *"   # 21:30 CST = 03:30 UTC
  workflow_dispatch:
jobs:
  ping:
    runs-on: ubuntu-latest
    steps:
      - name: Send WhatsApp via CallMeBot
        run: |
          curl -G "https://api.callmebot.com/whatsapp.php" \
            --data-urlencode "phone=${{ secrets.PHONE }}" \
            --data-urlencode "text=Hora de marcar los habitos del dia. Abre la app y check off lo de hoy." \
            --data-urlencode "apikey=${{ secrets.APIKEY }}"
```

Después en GitHub → Settings → Secrets → New repository secret:
- `PHONE` = `521<tunumero>`
- `APIKEY` = `<tu_apikey>`

Listo. Cada día a las 21:30 CST te llega WhatsApp.

### Alternativa local (LaunchAgent en la Mac, sólo si Mac está prendida)

Si no quieres GitHub, te armo un LaunchAgent. Pero si la Mac duerme/apaga, no dispara. Por eso recomiendo Actions.

## Datos

- localStorage key: `habitos_simone_v1`
- Estructura: `{ "YYYY-MM-DD": { "0": true, "5": true, ... } }` (índice 0-26 = índice de hábito en HABITS array)

## Tabla de hábitos (orden = orden del PDF original)

1. Tener pensamientos positivos
2. No tener pensamientos negativos
3. Leer 5 páginas de un libro
4. Verde
5. Dejar el cigarro
6. Crecimiento personal
7. Crecimiento profesional
8. Actualizar hoja gastos
9. Entrenar 1 vez
10. Entrenar 2 veces
11. Salir (no súper)
12. 7 min entrenamiento perros
13. Avanzar agencia inmobiliaria
14. Avanzar hidroponia
15. Meditación 1
16. Meditación 2
17. Peso
18. Explorar lugares nuevos
19. Desayuno
20. Comida
21. Cena
22. Bicicleta
23. Pendiente
24. Wingfoil
25. Mantenimiento casa
26. Ver un curso min. 30 minutos
27. Grabación de contenido
