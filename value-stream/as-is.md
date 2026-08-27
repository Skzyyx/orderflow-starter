# Value Stream AS-IS — Sprint 0

Documenten el flujo real desde el commit baseline hasta una ejecución verificable: trabajo, esperas, handoffs, feedback y fricciones observadas.

## Flujo observado

1. **Trabajo (Líder — José Luis Islas Molina)**: Creó el repositorio del equipo en GitHub, importó el starter y publicó el commit baseline "Initial Commit" (`0542e04`) en `main`. Esto correspondió al paso "Bootstrap del repositorio del equipo" indicado en el README.

2. **Handoff**: El líder compartió la URL del repositorio con el equipo para que otro integrante verificara la reproducibilidad desde un fresh clone.

3. **Trabajo (José Eduardo Aguilar Garcia)**: Realicé todo el resto del flujo de verificación:
   - Clon limpio del repositorio en mi máquina (`git clone`)
   - Confirmación del commit baseline (`git log --oneline -1`)
   - `mvn clean test` → tests verificados
   - `mvn package` → artifacts generados
   - Ejecución local del servicio (`mvn -pl orders-api spring-boot:run`)
   - Pruebas manuales de los tres endpoints (`health`, `POST /api/orders`, `GET /api/orders`)
     **Trabajo (Freddy Alí Castro Román)**: De igual manera realicé el flujo de verificación.
     **Trabajo (Christopher Alvarez Centeno)**: De igual manera realicé el flujo de verificación.

4. **Espera (José Eduardo Aguilar Garcia)**: Descarga de dependencias de Maven en la primera corrida (~30 segundos), al no tener el caché local previo.
   **Espera (Freddy Alí Castro Román)**: No tuve que realizar descargas extras, ya tenía todo instalado en el sistema.
   **Espera (Christopher Alvarez Centeno)**: No tuve que realizar descargas extras, ya tenía todo instalado en el sistema.

5. **Feedback (José Eduardo Aguilar Garcia)**: La terminal confirmó en cada paso el resultado (`BUILD SUCCESS`, tests pasados, respuestas JSON correctas de los endpoints), permitiendo validar el baseline sin ambigüedad.
   **Feedback (Freddy Alí Castro Román)**: De igual manera, la terminal confirmó resultados satisfactorios.
   **Feedback (Christopher Alvarez Centeno)**: Al igual que mis compañeros en la terminal confirmo resultados exitosos.

6. **Fricción (José Eduardo Aguilar Garcia)**: Al usar `Invoke-RestMethod` en `cmd.exe` (en vez de PowerShell), el comando no fue reconocido. Se resolvió usando el `curl` equivalente disponible en `cmd`.
   **Fricción (Freddy Alí Castro Román)**: No tuve problemas al momento de ejecutar los comandos puesto que ya estaba usando powershell 7.
   **Friccion (Christopher Alvarez Centeno)**: No tuve problemas al momento de correr los comandos en powershell.

## Fricciones identificadas

- Diferencia entre `cmd.exe` y PowerShell: `Invoke-RestMethod` solo funciona en PowerShell, y el README no especifica en qué terminal correr cada comando.
- Todo el trabajo de verificación recayó en una sola persona; no hubo doble-checking cruzado entre más integrantes en este sprint.
- Falta instalar maven en los equipos.

## Observaciones

- La división de responsabilidades fue clara: el líder cumplió el bootstrap según el README, y la verificación de reproducibilidad quedó a cargo de otro integrante, sin bloqueos de comunicación.
- El baseline resultó reproducible sin intervención del líder una vez compartida la URL del repo — no hubo dependencias ocultas ni pasos no documentados.
