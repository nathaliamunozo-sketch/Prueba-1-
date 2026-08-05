# Guía de pruebas: alternativas gratis a Claude Code en terminal

Esta guía es para probar, antes de llevarlo al diplomado, caminos gratuitos que dan una experiencia de "vibe coding" en terminal parecida a Claude Code. La idea es familiarizar a los participantes con el flujo de terminal sin pedirles pagar nada, y que la transición a Claude Code después sea natural.

> **Corrección (2026-07-28):** el "GLM Coding Plan" de Z.ai que se consideró primero **no es gratis**. El plan más barato ("Lite") cuesta $12,6/mes facturado anual o $18/mes mensual — no existe nivel gratuito real, solo una "cuota básica incluida" dentro de un plan pago. Se descarta para el diplomado y se reemplaza por Gemini CLI abajo.

## Camino 1: Gemini CLI (recomendado, gratis real)

Oficial de Google (`github.com/google-gemini/gemini-cli`), agéntico igual que Claude Code: edita archivos del proyecto, corre comandos, todo desde lenguaje natural en la terminal. Se autentica con una cuenta normal de Google, sin tarjeta, y el nivel gratis tiene una cuota diaria generosa.

### Qué hacer (una sola vez, por persona)

1. Instalar: `npm install -g @google/gemini-cli`
2. Correr `gemini` en cualquier carpeta de proyecto.
3. La primera vez pide iniciar sesión con una cuenta de Google (navegador se abre solo) — no requiere tarjeta ni API key manual.

### Notas

- El límite gratis es por cuenta de Google y se resetea diario.
- Calidad comparable a lo que daba Mimo Code, mejor con prompts detallados y específicos, igual que Claude Code.

## Camino 2: Aider

CLI independiente, open source, con años de desarrollo y buena adopción. No imita la interfaz de Claude Code, pero el flujo de fondo es el mismo: le describes en lenguaje natural qué necesitas, edita archivos del proyecto, y hace commits.

### Instalación (ya hecha en esta máquina para pruebas)

```powershell
python -m pip install --user aider-install
aider-install
```

### Qué hacer para usarlo gratis

Aider necesita un modelo detrás. Vías sin costo:

- **Con la API gratuita de Gemini** (misma cuenta del Camino 1): `aider --model gemini/gemini-2.5-pro` usando una API key gratuita generada en [aistudio.google.com](https://aistudio.google.com/apikey).
- **100% local, sin cuenta**: instalar [Ollama](https://ollama.com), bajar un modelo de código (ej. `ollama pull qwen2.5-coder`), y correr `aider --model ollama/qwen2.5-coder`. Más lento y menos potente, pero sin depender de ningún servicio externo ni límite de cuota.

## Camino 3: OpenCode

CLI agéntica open source (`opencode.ai`), con una experiencia de terminal muy similar a Claude Code. Al instalarla, permite usar agentes gratis sin necesidad de pagar ni de traer su propia clave de API. Probada y confirmada como segura para este uso.

### Instalación

```bash
npm install -g opencode-ai
```

Correrla siempre desde la carpeta específica del proyecto del participante (nunca desde una carpeta raíz o compartida), para evitar que el agente toque archivos fuera del alcance de su propio sistema.

### Cuándo recomendarla en vez de Gemini CLI

Como alternativa cuando alguien ya tiene VS Code instalado y prefiere mantenerse en terminal sin salir a una extensión, o cuando la cuota diaria de Gemini CLI se agota a mitad de sesión con un grupo grande.

## Camino 4 (opcional, de pago barato): Claude Code + GLM

Si en algún momento se decide que vale la pena pagar un plan barato en vez de usar solo herramientas 100% gratis, queda listo el alias `glm` en el perfil de PowerShell — reutiliza el mismo binario `claude`, apuntado al modelo GLM-4.6 de Z.ai. Requiere suscribirse al plan "Lite" ($12,6-18/mes) y generar una API key, guardada en `C:\Users\[usuario]\.glm\api_key.txt`. No se recomienda para el diplomado mientras existan opciones gratis reales como Gemini CLI.

## Qué probar antes de llevarlo al diplomado

- [ ] Que `gemini` autentique sin fricción con una cuenta de Google nueva (probar con una cuenta que no sea la personal, para simular la de un participante).
- [ ] Pedirle una tarea real de código (ej. crear un componente simple) y comparar calidad de respuesta contra lo que daba Mimo.
- [ ] Repetir la misma prueba con Aider + Gemini.
- [ ] Confirmar cuánto dura la cuota gratis diaria de Gemini con uso normal de una sesión de clase, para saber si alcanza para todos los participantes en un mismo día.
