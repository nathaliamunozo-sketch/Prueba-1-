# Adición del día: construir y conectar su sistema (Sesión 3)

Este documento es NUEVO, no reemplaza `GUIA-DESPLIEGUE-PARTICIPANTES.md` que ya recibieron — se entrega junto a esa guía, no en vez de ella. Cubre lo que faltó: instalar OpenCode paso a paso, usar Supabase de verdad (no solo tener la cuenta), y un prompt ya armado para que cualquiera, sepa o no "pedir bien", pueda construir su sistema.

Código de colores de esta guía (igual en todo el material del módulo):

<div style="border-left:5px solid #16A34A;background:#F0FDF4;padding:8px 14px;margin:6px 0"><strong style="color:#15803D">VERDE = Supabase</strong></div>
<div style="border-left:5px solid #F97316;background:#FFF7ED;padding:8px 14px;margin:6px 0"><strong style="color:#C2410C">NARANJA = Vercel</strong></div>
<div style="border-left:5px solid #64748B;background:#F1F5F9;padding:8px 14px;margin:6px 0"><strong style="color:#334155">GRIS AZULADO = Git / GitHub</strong></div>
<div style="border-left:5px solid #9333EA;background:#FAF5FF;padding:8px 14px;margin:6px 0"><strong style="color:#7E22CE">PÚRPURA = consejos para leer SU propio dashboard</strong></div>

---

## 1. Instalar OpenCode, desde cero, sin dar nada por sabido

<div style="border-left:5px solid #64748B;background:#F1F5F9;padding:10px 16px;margin:10px 0">
<strong style="color:#334155">GIT / GITHUB</strong> — este paso vive en terminal, la misma herramienta que después usa para subir su código.
</div>

OpenCode necesita una sola cosa instalada antes: **Node.js**. Node no es OpenCode — es el motor que OpenCode necesita para correr, como necesitar gasolina antes de poder manejar un carro.

**Paso 0: revise si ya tiene Node instalado.**

1. Abra una terminal: en Windows, busque **"Símbolo del sistema"** o **"PowerShell"** en el menú de inicio y ábralo.
2. Escriba exactamente `node -v` y presione Enter.
3. Si le sale algo como `v20.11.0` (un número de versión), **ya tiene Node** — salte directo al "Paso 2".
4. Si le sale un error tipo "no se reconoce el comando" o "not recognized", no tiene Node — siga con el "Paso 1".

**Paso 1: instalar Node (solo si el Paso 0 dio error).**

1. Vaya a **[nodejs.org](https://nodejs.org)** en su navegador.
2. Va a ver dos botones grandes para descargar. Clic en el que diga **"LTS"** (no el que diga "Current").
3. Se descarga un archivo `.msi`. Ábralo con doble clic.
4. Se abre un instalador: **Next** → aceptar los términos (marcar la casilla) → **Next** → **Next** → **Install**. No cambie ninguna opción, lo que trae por defecto está bien.
5. Clic en **Finish** al terminar.
6. **Cierre la terminal que tenía abierta y ábrala de nuevo.** Esto es importante: si no la cierra y la vuelve a abrir, no se entera de que Node ya quedó instalado.
7. Repita el Paso 0 (`node -v`) para confirmar que ahora sí muestra un número de versión.

**Paso 2: instalar OpenCode.**

1. En la terminal, escriba exactamente `npm install -g opencode-ai` y presione Enter. Va a ver texto corriendo unos segundos — es normal, está descargando.
2. Cuando termine, escriba `opencode -v` para confirmar que quedó instalado.
3. **Muy importante:** antes de usarlo, navegue hasta la carpeta de SU proyecto con el comando `cd` (ejemplo: `cd Desktop\mi-sistema-evaluativo`). Nunca lo corra desde el Escritorio directamente ni desde una carpeta general — siempre desde adentro de la carpeta específica de su proyecto.
4. Ya adentro de esa carpeta, escriba `opencode` y presione Enter. Se abre un chat dentro de la misma terminal: ahí le escribe en español lo que necesita.

---

## 2. Usar Supabase de verdad (no solo tener la cuenta)

<div style="border-left:5px solid #16A34A;background:#F0FDF4;padding:10px 16px;margin:10px 0">
<strong style="color:#15803D">SUPABASE</strong> — esto es lo que faltaba: ya tienen la cuenta, les falta usarla.
</div>

Tener la cuenta creada no es lo mismo que tener su dato guardándose ahí. Estas son las 2 pantallas de Supabase que van a usar hoy:

**Table Editor (para VER sus datos):** en el menú de la izquierda de su proyecto Supabase, clic en el ícono de tabla ("Table Editor"). Ahí aparece, en filas y columnas como un Excel, cada respuesta que llega desde su formulario. Si su tabla no aparece en esta lista, es porque todavía no la ha creado (siguiente punto) o porque su HTML no se está conectando bien a Supabase.

**SQL Editor (para CREAR su tabla):** en el mismo menú, clic en el ícono de rayo o `</>` ("SQL Editor"). Aquí es donde pega el código SQL que su IA le genera para crear la tabla — no lo escribe usted a mano, se lo pide a su IA (ver el prompt de la sección 3). Clic en **Run** (o `Ctrl+Enter`) para ejecutarlo. Si sale un error en rojo abajo, cópiele ese error exacto a su IA y pídale que lo corrija.

**Dónde está la URL y la llave que su HTML necesita:** menú **Project Settings** (el ícono de engranaje, abajo a la izquierda) → **API**. Ahí están el **Project URL** y la **anon public key** — cópielos y péguelos donde su IA le indique dentro de su archivo HTML.

---

## 3. El prompt maestro: llénelo y péguelo completo

Este es el problema real que vimos: solo uno del grupo logró construir bien su sistema, porque supo darle instrucciones claras a su IA. Este prompt existe para que **cualquiera**, sepa o no "pedir bien", pueda lograrlo — solo hay que llenar los espacios en corchetes `[ ]` con su información y pegarlo completo, sin editar el resto.

La parte más importante de este prompt no es el código que pide: es que **le exige a la IA que primero le repita en español simple qué entendió, ANTES de escribir cualquier archivo.** Así, si la IA entendió mal algo, usted lo nota en 10 segundos leyendo un párrafo — no 20 minutos después revisando código que no sirve.

> Necesito que construyas un sistema evaluativo simple para mi clase de **[su disciplina, ej: enfermería]**.
>
> Mi instrumento mide estos bloques o indicadores: **[pegue aquí sus 3-4 indicadores del taller de la Sesión 2, tal como los escribió — ej: "% que aplica el protocolo correcto ante un caso nuevo", "% que identifica los 5 pasos del lavado de manos en orden"]**.
>
> Las preguntas exactas de mi instrumento son: **[pegue aquí las preguntas de su cuestionario, con sus opciones de respuesta si las tiene]**.
>
> Necesito 2 archivos: `index.html` (el formulario que responde el estudiante, que guarda cada respuesta en una tabla de Supabase) y `facilitador.html` (donde yo veo un resumen de las respuestas: promedio por indicador, y una lista de quién respondió qué).
>
> Usa solo HTML, CSS y JavaScript plano — sin ningún framework, sin paso de instalación (`npm run build` u otro).
>
> **Antes de escribir ningún archivo, respóndeme primero en un párrafo corto, en español simple: qué vas a construir, qué columnas va a tener mi tabla de Supabase, y cómo se conecta cada pregunta de mi instrumento con esas columnas. Espera mi confirmación de que eso está bien antes de generar el código.**
>
> Cuando yo confirme, genera primero el SQL de la tabla de Supabase, y después los 2 archivos HTML. Explícame en español simple qué hace cada parte.

**Qué hacer con la respuesta de la IA:**
1. Lea el párrafo de confirmación. Si algo no coincide con lo que usted quiso decir (le faltó un indicador, entendió mal una pregunta), corríjala ahí mismo antes de dejarla seguir: "no, el segundo indicador es sobre X, no sobre Y."
2. Cuando esté de acuerdo, dígale "sí, procede" (o similar) para que genere el SQL y los archivos.
3. Pegue el SQL que le dio en el **SQL Editor** de Supabase (sección 2 de arriba) y ejecútelo.
4. Pruebe el `index.html` en su navegador, revise en **Table Editor** que la respuesta sí llegó.

---

## 4. Consejos para leer su propio dashboard

<div style="border-left:5px solid #9333EA;background:#FAF5FF;padding:10px 16px;margin:10px 0">
<strong style="color:#7E22CE">CONSEJO</strong>
</div>

- Un dashboard vacío al principio es normal — no significa que esté roto, significa que todavía nadie ha respondido. Pruébelo usted mismo respondiendo su propio formulario 2-3 veces para ver el patrón funcionar antes de dárselo a sus estudiantes.
- Si un número no le dice nada útil a simple vista, pídale a su IA: "agrégale una frase debajo de este número que explique qué significa en palabras simples."
- No busque perfección visual. El objetivo de hoy es que el dato de sus estudiantes se convierta en algo que usted pueda leer y usar para decidir — no un dashboard bonito.

---

## Si algo no funciona

Mismo consejo de siempre: copie el mensaje de error exacto (no "no funciona") y péguelo en su IA junto con qué estaba intentando hacer. Los 3 errores más comunes hoy van a ser: la llave de Supabase mal copiada, la tabla SQL no creada todavía en Supabase, o un nombre de columna que no coincide entre el HTML y la tabla.
