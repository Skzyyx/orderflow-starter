# Value Stream TO-BE — Sprint 0

Propongan el flujo objetivo, indiquen qué automatizarían primero y justifiquen la decisión con evidencia del AS-IS.

## Flujo objetivo

1. El líder publica el commit baseline en `main` (sin cambios respecto al AS-IS, ya que este paso no presentó fricciones).
2. Se agrega el Maven Wrapper al repositorio (`mvn wrapper:wrapper`, commiteando `mvnw`, `mvnw.cmd` y `.mvn/`), de forma que todo el equipo compile con la misma versión de Maven sin necesidad de instalarlo localmente. La checklist de entorno se reduce a: Java 21, Git y PowerShell 7.
3. Se estandariza el trabajo en PowerShell 7 (`pwsh`), no solo porque evitó la fricción observada en el AS-IS, sino porque es multiplataforma (Windows/Mac/Linux usan el mismo shell) y es el shell que usan por defecto los runners de GitHub Actions (`shell: pwsh`). Esto asegura que cualquier script de verificación escrito ahora corra sin cambios en el CI de la Semana 3.
4. Se crea un script simple `check-env.ps1` que imprime las versiones de Java, Git y del wrapper de Maven, reemplazando la verificación manual de cada integrante por un solo comando.
5. Los resultados de build, tests y endpoints se registran en `docs/evidence/sprint-XX.md` conforme se obtienen, en lugar de documentarse todo al final.

## Qué automatizaríamos primero

**Verificación automática del baseline en cada push**, mediante un pipeline de CI que incluya:
- `./mvnw verify` (build + test + package, usando el wrapper para fijar la versión de Maven)
- Un **smoke test**: levantar la aplicación y hacer una petición real a `/actuator/health` y a `/api/orders`, confirmando que los endpoints responden — no solo que el código compila.

Esto no se implementa en este Sprint porque el README indica explícitamente que `.github/workflows` se desarrolla progresivamente y no debe adelantarse. Pero el Maven Wrapper y el script `check-env.ps1` sí se pueden agregar en este Sprint, sin tocar esa carpeta, y dejan el terreno listo para que el pipeline de la Semana 3 use exactamente los mismos comandos.

## Justificación con evidencia del AS-IS

- **Fricción observada**: `Invoke-RestMethod` falló en `cmd.exe` para José Eduardo, mientras que Freddy y Christopher no tuvieron ese problema por ya usar PowerShell. La evidencia del AS-IS respalda concretamente "no usar cmd" — no distingue entre versiones de PowerShell, ya que `Invoke-RestMethod` existe desde PowerShell 3. Se elige PowerShell 7 en particular porque es multiplataforma y es el shell nativo de GitHub Actions, lo cual conecta esta decisión directamente con el pipeline que se implementará más adelante.
- **Fricción observada**: falta de Maven instalado en algunos equipos. Esto no lo resuelve un pipeline de CI (que solo estandariza la verificación oficial en el servidor), sino el **Maven Wrapper**, que elimina la necesidad de instalar Maven en cualquier máquina local, incluyendo las de quienes solo quieren compilar o correr la app sin depender de un pipeline remoto.
- **Fricción observada**: la verificación manual de endpoints (`curl`/`Invoke-RestMethod`) dependió del entorno de cada integrante. Un pipeline que solo compile y empaquete no elimina esta fricción — se elimina únicamente si el pipeline incluye un paso de smoke test que levante la aplicación y pruebe los endpoints automáticamente, tal como se especifica en la sección anterior.
- **Observación del AS-IS**: tres integrantes verificaron el fresh clone de forma independiente y todos obtuvieron resultados consistentes, a pesar de tener configuraciones de entorno distintas. Esto confirma que el baseline es reproducible entre máquinas, y que las mejoras propuestas (wrapper, script de verificación, smoke test) atacan la variabilidad de entorno restante sin necesitar aún el pipeline formal.

En resumen: el pipeline de la Semana 3 elimina la variabilidad de entorno para la verificación oficial del baseline; mientras tanto, el Maven Wrapper elimina la dependencia de tener Maven instalado localmente, y el script de verificación elimina la dependencia de que cada integrante revise su entorno a mano.