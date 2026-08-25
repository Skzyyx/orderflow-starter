# Value Stream AS-IS — Sprint 0

Documenten el flujo real desde el commit baseline hasta una ejecución verificable: trabajo, esperas, handoffs, feedback y fricciones observadas.

## Flujo observado

1. **Trabajo (Líder — Skzyyx)**: Creó el repositorio del equipo en GitHub, importó el starter y publicó el commit baseline "Initial Commit" (`0542e04`) en `main`. Esto correspondió al paso "Bootstrap del repositorio del equipo" indicado en el README.

2. **Handoff**: El líder compartió la URL del repositorio con el equipo para que otro integrante verificara la reproducibilidad desde un fresh clone.

3. **Trabajo (Jose Aguilar)**: Realicé todo el resto del flujo de verificación:
   - Clon limpio del repositorio en mi máquina (`git clone`)
   - Confirmación del commit baseline (`git log --oneline -1`)
   - `mvn clean test` → tests verificados
   - `mvn package` → artifacts generados
   - Ejecución local del servicio (`mvn -pl orders-api spring-boot:run`)
   - Pruebas manuales de los tres endpoints (`health`, `POST /api/orders`, `GET /api/orders`)

4. **Espera**: Descarga de dependencias de Maven en la primera corrida (~30 segundos), al no tener el caché local previo.

5. **Feedback**: La terminal confirmó en cada paso el resultado (`BUILD SUCCESS`, tests pasados, respuestas JSON correctas de los endpoints), permitiendo validar el baseline sin ambigüedad.

6. **Fricción**: Al usar `Invoke-RestMethod` en `cmd.exe` (en vez de PowerShell), el comando no fue reconocido. Se resolvió usando el `curl` equivalente disponible en `cmd`.

## Fricciones identificadas
- Diferencia entre `cmd.exe` y PowerShell: `Invoke-RestMethod` solo funciona en PowerShell, y el README no especifica en qué terminal correr cada comando.
- Todo el trabajo de verificación recayó en una sola persona; no hubo doble-checking cruzado entre más integrantes en este sprint.

## Observaciones
- La división de responsabilidades fue clara: el líder cumplió el bootstrap según el README, y la verificación de reproducibilidad quedó a cargo de otro integrante, sin bloqueos de comunicación.
- El baseline resultó reproducible sin intervención del líder una vez compartida la URL del repo — no hubo dependencias ocultas ni pasos no documentados.