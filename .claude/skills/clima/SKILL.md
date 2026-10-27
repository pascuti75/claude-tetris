---
name: clima
description: Obtiene el clima actual y el pronóstico de los próximos días para Sevilla, España. Usar cuando pida "clima", "tiempo", "temperatura", "va a llover", "pronóstico" o /clima.
allowed-tools: Bash(curl *)
---

# Clima de Sevilla

Consulta el clima con [wttr.in](https://wttr.in) (sin API key). La ciudad es siempre Sevilla, España; ignora cualquier otra ciudad en los argumentos.

## Pasos

1. Ejecuta (Bash, `curl` viene con Windows 10/11):

   ```bash
   curl -s "https://wttr.in/Sevilla,Spain?format=j1&lang=es"
   ```
2. Del JSON usa:
   - Ahora: `current_condition[0]` → `temp_C`, `FeelsLikeC`, `humidity`, `windspeedKmph`, `winddir16Point`, `lang_es[0].value` (descripción)
   - Pronóstico: `weather[]` (3 días) → `date`, `mintempC`, `maxtempC`, `hourly[4].lang_es[0].value`, `hourly[4].chanceofrain`
3. Responde en español, conciso: condición actual y una línea por día de pronóstico. Avisa si hay probabilidad de lluvia alta (>50%).

## Fallo

Si `curl` falla o devuelve algo que no es JSON, reintenta una vez; si persiste, dilo.
