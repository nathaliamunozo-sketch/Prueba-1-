# Arquitectura de referencia para el sistema evaluativo del participante

Este documento no es para el profesor participante. Es para la IA que él o ella use (Claude Code, GitHub Copilot, Cursor, Gemini Code Assist, o cualquier otra). Se pega al inicio de la conversación con esa IA, o se deja como archivo `CONTEXTO.md` en la raíz de su proyecto, para que la IA entienda de entrada qué se está construyendo, con qué herramientas, y qué NO hace falta construir.

Si usted es la IA leyendo esto: el usuario es un profesor universitario sin experiencia previa de programación, guiado por un diplomado de 4 sesiones. Priorice simplicidad y explicaciones sobre elegancia técnica. No proponga arquitecturas alternativas a las de este documento salvo que el usuario lo pida explícitamente.

---

## 1. Qué se está construyendo (el MVP mínimo)

Un sistema evaluativo propio, con 3 piezas obligatorias:

1. **Diagnóstico propio aplicado**: un instrumento (cuestionario) que el profesor diseñó y aplicó a sus estudiantes reales, o a un piloto representativo.
2. **Analítica de esos datos**: un dashboard simple que toma esas respuestas y las convierte en algo interpretable (promedios, niveles, una gráfica), no solo una lista de números.
3. **Mapeo de las 4 temáticas del diplomado a las 4 sesiones**: el sistema conecta con claridad qué bloque de su instrumento corresponde a qué sesión (evaluación auténtica, uso de datos, evaluación adaptativa, ética del uso de IA).

Todo lo que esté más allá de estas 3 piezas (lista de estudiantes con roster, planeación de contenidos, analítica continua en el tiempo, detección de uso de IA) es opcional y suma como mérito adicional, nunca es requisito. No proponga estas extensiones de entrada; constrúyalas solo si el usuario las pide.

## 2. Stack elegido, y por qué

| Pieza | Herramienta | Por qué esta y no otra |
|---|---|---|
| Control de versiones | GitHub | Ya tiene cuenta creada desde la Sesión 1. Es el estándar que cualquier asistente de IA conoce mejor. |
| Hosting | Vercel | Despliegue de un sitio estático sin configuración: conectar el repositorio y ya. No necesita servidor propio ni Docker. |
| Base de datos y autenticación | Supabase | Postgres real con una capa de API HTTP y autenticación lista para usar, sin tener que escribir un backend. Tiene plan gratis suficiente para este tamaño de proyecto. |
| Frontend | HTML + CSS + JavaScript plano, sin framework ni paso de build | Nada que compilar, nada que romperse por versiones de dependencias. Un profesor sin experiencia previa puede abrir el archivo y entender qué hace cada parte. |

Esta combinación no es la única buena, ni necesariamente la más potente. Existen alternativas serias y en algunos casos técnicamente superiores para ciertos casos de uso: **Neon** en vez de Supabase (Postgres serverless, más flexible para quien ya sabe SQL avanzado), **Cloudflare Pages/Workers** en vez de Vercel (más rápido y barato a gran escala, con más piezas que configurar). Se eligió GitHub + Vercel + Supabase porque es la combinación con más documentación, más soporte nativo en asistentes de IA, y menor curva de aprendizaje para alguien que nunca ha desplegado nada. Si el usuario ya tiene experiencia y prefiere otra combinación, es una decisión válida, pero no es la que este módulo enseña por defecto.

**Sobre el frontend en HTML plano, específicamente**: esta elección es un ejercicio didáctico, no un techo técnico. HTML + CSS + JavaScript sin paso de build es el punto de entrada más bajo para alguien sin experiencia previa: no hay que instalar Node, no hay `node_modules`, no hay que entender qué es un bundler antes de ver algo funcionando en el navegador. El propósito de este módulo es que el participante entienda el patrón (sección 4) y lo pueda defender con su propio código, no que memorice una sintaxis de framework.

Una vez el participante domina ese patrón, migrar a un framework (React, Vue, Svelte) con su herramienta de build (Vite, Next.js) es un paso natural y deseable para un sistema que va a usarse en el día a día: mejor rendimiento, componentes reutilizables, mejor manejo de estado, y un ecosistema de librerías que en HTML plano hay que reinventar a mano. Esa migración trae consigo la instalación de nuevas dependencias, un paso de compilación antes de desplegar, y una curva de aprendizaje adicional; son costos reales, no gratuitos, y por eso no se enseñan en este módulo de 4 sesiones. Este documento y el MVP que describe son el abrebocas: la puerta de entrada para que el participante seguidamente explore por su cuenta, o en un módulo posterior, herramientas más eficientes para producción.

**Ruta avanzada, solo si el participante ya tiene la herramienta:** quien ya cuenta con una suscripción a un asistente de IA más capaz (Claude Code, Codex, Antigravity, o un plan de pago de OpenCode) puede saltarse el punto de entrada en HTML plano y construir directamente en Node.js, Next.js con TypeScript, Django, o una pila equivalente. La razón para no exigirlo por defecto es la misma de arriba: esas herramientas absorben el costo extra (dependencias, build, curva de aprendizaje) mejor que un asistente gratuito con cuota limitada, pero no todos los participantes las tienen. Si alguien ya la tiene, no hay que frenarlo hacia HTML plano — que use lo que ya paga.

## 3. Especificación mínima de datos

Como mínimo, el sistema necesita una tabla (o vista equivalente) con:

- Una fila por respuesta al instrumento.
- Un campo de fecha/hora de creación.
- Un campo por cada bloque temático que el instrumento mida (puntaje numérico).
- Un nivel o categoría derivada de esos puntajes (ej. "inicial", "en desarrollo", "consolidado").
- Un identificador de cohorte, si el instrumento se va a aplicar a más de un grupo a través del tiempo.

No hace falta normalizar en múltiples tablas relacionadas para un MVP de este tamaño: una sola tabla con una columna JSON para las respuestas crudas (como hace el diagnóstico de referencia de este módulo) es suficiente y más fácil de enseñar.

## 4. Patrón general (enseñado en la Sesión 1)

```
Instrumento → Captura de datos → KPIs → Interpretación → Motor de reglas → Retroalimentación → Dashboard → Plan de acción
```

Cualquier sistema que construya el participante, sin importar el dominio de su disciplina, debería poder explicarse siguiendo este mismo flujo. Si al revisar el código de un participante este patrón no es identificable, es una señal de que el diseño necesita simplificarse, no de que necesita más funciones.

## 5. Bloques opcionales (solo si el participante los pide o ya tiene el MVP mínimo resuelto)

Estos no son parte del MVP mínimo. Constrúyalos únicamente si el participante ya completó las 3 piezas obligatorias de la sección 1 y quiere ir más allá:

- **Ingesta desde Excel**: si el profesor ya tiene datos de sus estudiantes en un archivo `.xlsx` (notas, asistencia), se puede escribir un script corto en Python (librería `openpyxl` o `pandas`) que lea ese archivo y lo cargue a la tabla de Supabase, en vez de que el profesor tenga que capturar todo de nuevo a mano.
- **Automatización con una API de IA gratuita**: para generar retroalimentación automática sin depender de copiar y pegar en un chat, se puede conectar una API de inferencia con capa gratuita directamente desde el backend de Supabase (Edge Functions) o desde un flujo de automatización tipo n8n. Dos opciones confirmadas: **Gemini** (Google AI Studio, ver `GUIA-DESPLIEGUE-PARTICIPANTES.md` para el paso a paso de crear la clave con una cuenta de Google personal, no la institucional de Unicauca) y **Groq** (API de inferencia rápida sobre modelos abiertos, también con capa gratuita). Groq no es un asistente de código para el IDE como Gemini Code Assist — sirve únicamente para este caso de automatización de backend, no para reemplazar la herramienta con la que el docente programa. Esto es un plus de independencia técnica, no algo que se enseñe ni se exija en este módulo.
- **Roster de estudiantes y seguimiento en el tiempo**: una segunda tabla `estudiantes` o `participantes` con seguimiento sesión a sesión, siguiendo el mismo patrón que usa el dashboard de la facilitadora del diplomado (ver `diagnostico-app/facilitador.html` como referencia de implementación, no como algo que el participante deba copiar literalmente).

## 6. Qué NO hacer

- No proponga un framework de frontend (React, Vue, Next.js) salvo que el participante ya lo conozca y lo pida explícitamente. Complica el despliegue sin aportar valor a este tamaño de proyecto.
- No proponga autenticación compleja (roles múltiples, OAuth con terceros) si el sistema solo lo va a usar el propio profesor. Un login simple de Supabase Auth con una sola cuenta es suficiente.
- No genere código sin explicar, en español simple, qué hace cada parte. El objetivo pedagógico del módulo es que el profesor entienda su propio sistema, no que reciba una caja negra funcional.
