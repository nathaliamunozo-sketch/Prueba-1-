# Guía de despliegue para su propio sistema

Esta guía es para usted, profesor participante del diplomado, no para la facilitadora. Le sirve para publicar SU propio sistema evaluativo (el que diseña y construye en las Sesiones 2 y 3), usando las 3 cuentas gratuitas que ya creó al terminar la Sesión 1.

Si todavía no ha creado sus cuentas de GitHub, Vercel y Supabase, hágalo antes de seguir (ver "Preparando su herramienta" en su guía de la Sesión 1).

---

## 1. Elija con qué IA va a construir

No necesita pagar nada para hacer esto. La opción confirmada con API gratuita real es **Gemini**, de Google:

| Herramienta | Costo | Cómo empezar |
|---|---|---|
| **Gemini** (API gratis) | Gratis, con límite diario de usos | Extensión "Gemini Code Assist" en VS Code (inicia sesión con una cuenta de Google, sin clave manual), o una clave de API propia para herramientas de terminal como Aider (ver instrucciones abajo). |
| **Claude Code** | De pago (suscripción) | La opción más capaz, pero no es necesaria para el MVP mínimo de este módulo. Si ya la usa y le sirve, adelante. |

Otras herramientas (GitHub Copilot, Cursor, GLM/Z.ai) fueron evaluadas para este módulo, pero al momento de escribir esta guía ya no ofrecen acceso gratuito por API que se pueda conectar a herramientas de terminal, solo uso dentro de su propia interfaz o con límites que cambian seguido. Confirme la oferta vigente antes de recomendar una distinta a Gemini.

### Cómo crear su clave de API de Gemini (gratis)

Solo hace falta si va a usar una herramienta de terminal (como Aider) o si su propio sistema va a llamar a la API de Gemini directamente (ver `ARQUITECTURA-REFERENCIA.md`, sección 5). Si solo va a usar la extensión "Gemini Code Assist" en VS Code, no necesita esto: esa extensión inicia sesión directo con su cuenta de Google, sin clave manual.

1. **Use una cuenta de Gmail personal, no la institucional de Unicauca.** Las cuentas de Google Workspace institucionales casi siempre tienen bloqueada la creación de claves de API por política del administrador de la organización. Si intenta con su correo `@unicauca.edu.co` y la página no lo deja avanzar, ese es el motivo: cambie a su cuenta personal.
2. Vaya a **[aistudio.google.com/apikey](https://aistudio.google.com/apikey)** e inicie sesión con esa cuenta personal.
3. Clic en **Create API key** (o **Get API key** → **Create API key in new project**, si es la primera vez).
4. Google genera una clave que empieza con `AIza...`. Clic en el ícono de copiar junto a la clave.
5. Guarde esa clave en un lugar seguro (un gestor de contraseñas, o una nota privada). **Nunca la pegue en un archivo HTML ni la suba a GitHub** — a diferencia de la llave pública de Supabase, esta sí es secreta.
6. Dónde pegarla, según la herramienta:
   - **Aider**: al ejecutarlo, `aider --model gemini/gemini-2.5-flash --api-key gemini=SU_CLAVE_AQUI`, o guardarla una sola vez como variable de entorno (`GEMINI_API_KEY`) para no repetirla cada vez.
   - **Su propio sistema** (si automatiza retroalimentación con IA desde Supabase u otro backend): como variable de entorno del lado del servidor (ej. un secreto de Supabase Edge Functions), nunca en el código que corre en el navegador del estudiante.

## 2. Cree su proyecto en Supabase

1. Entre a [supabase.com](https://supabase.com) con la cuenta que ya creó.
2. **New project** → nombre (ej. `mi-sistema-evaluativo`), región más cercana (para Colombia: `South America (São Paulo)`), y una contraseña de base de datos (guárdela).
3. Espere 1-2 minutos a que quede `ACTIVE_HEALTHY`.
4. En **SQL Editor**, pídale a su asistente de IA que le genere el SQL de su tabla siguiendo la especificación mínima de datos de `ARQUITECTURA-REFERENCIA.md` (sección 3). No copie el SQL de la facilitadora tal cual: el suyo depende de las preguntas y bloques específicos de SU instrumento.
5. En **Project Settings → API**, copie el **Project URL** y la **anon public key**. No son secretos, están hechos para vivir en el navegador.

## 3. Suba su código a GitHub

1. En su editor (VS Code con Gemini Code Assist, u otro), pídale a su IA que inicialice un repositorio Git en la carpeta de su proyecto.
2. Cree un repositorio nuevo y vacío en [github.com](https://github.com) (botón **New**).
3. Conecte y suba su código (su IA puede ejecutar estos comandos por usted si se lo pide: "conecta este proyecto al repositorio de GitHub que acabo de crear y sube el código").

## 4. Publique en Vercel

1. En [vercel.com](https://vercel.com), **New Project** → importe el repositorio que acaba de subir.
2. Framework: **Other** (sitio estático), a menos que su IA haya construido algo distinto y se lo indique.
3. **Deploy**. Su sistema queda en una URL como `https://su-proyecto.vercel.app`.
4. Cada vez que suba cambios nuevos a GitHub, Vercel los publica solo, sin que usted tenga que repetir este paso.

## 5. Antes de la Sesión 4

Su sistema debe estar publicado (con una URL real, no solo funcionando en su computador) y con datos reales o de un piloto ya cargados. La Sesión 4 es de validación cruzada con un compañero de otra disciplina: necesita que su sistema sea accesible desde cualquier navegador, no solo desde el suyo.

## Si algo no funciona

Copie el mensaje de error exacto y péguelo en su asistente de IA junto con la pregunta "¿por qué está pasando esto y cómo lo soluciono?". La mayoría de errores en este tipo de proyecto son de uno de estos 3 tipos: la llave de Supabase mal copiada, una política de seguridad (RLS) que bloquea una consulta, o un nombre de columna que no coincide entre el código y la tabla. Describir el error completo a la IA, en vez de solo decir "no funciona", ahorra la mayor parte del tiempo de depuración.
