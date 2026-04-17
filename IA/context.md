Basándome en el análisis completo del archivo `index.html`, aquí está el contenido para guardar en `IA/context.md`:

```markdown
# context.md — Proyecto: FuntionalQAPromtGenerate

> **Última actualización:** 2026 — Generado automáticamente por análisis estático del proyecto.

---

## 1. Resumen

`FuntionalQAPromtGenerate` es una aplicación **web estática de página única (SPA)** desarrollada en HTML5, CSS3 y JavaScript vanilla (sin frameworks ni dependencias externas). Su propósito es permitir a QA Engineers configurar un stack tecnológico de automatización de pruebas y generar automáticamente un **prompt de alta calidad** listo para ser usado en herramientas de IA (ChatGPT, Copilot, Claude, etc.), que a su vez genera un proyecto completo de automatización funcional BDD.

La aplicación no requiere servidor backend, base de datos ni proceso de build. Es completamente ejecutable desde el navegador como archivo estático.

---

## 2. Arquitectura y Módulos

### Tipo de arquitectura
- **Frontend estático puro**: un único archivo `index.html` autocontenido.
- Sin dependencias externas (sin npm, sin CDN, sin librerías JS de terceros).
- Sin backend, sin API calls, sin almacenamiento persistente.

### Módulos lógicos internos (JavaScript)

| Módulo | Función |
|---|---|
| **UI Helpers** | Control del acordeón, tabs, alertas, cambios de estado en el DOM |
| **Config Reader** (`getConfig()`) | Lee todos los inputs del formulario y retorna objeto de configuración |
| **Version Resolvers** | `getBDDVersion()`, `getRunnerVersion()`, `getUIToolVersion()` — determinan versiones según selección |
| **Display Formatters** | `getLangDisplay()`, `getBuildDisplay()`, `getAppTypeDisplay()`, etc. — transforman valores internos a etiquetas legibles |
| **Prompt Builder** (`buildPrompt()`) | Orquesta la construcción del prompt dividido en secciones |
| **Section Builders** | `buildJavaDepsSection()`, `buildPythonDepsSection()`, `buildOutputSection()`, `buildBestPracticesSection()` |
| **Render Engine** (`renderOutput()`) | Muestra el prompt generado en las pestañas Preview, Raw y Resumen |
| **Markdown Renderer** (`markdownToHtml()`) | Convierte Markdown a HTML para la vista previa (renderer propio sin librerías) |
| **Clipboard Handler** (`copyPrompt()`) | Copia el prompt al portapapeles con fallback para entornos sin API Clipboard |

### Flujo principal
```
Usuario completa formulario
↓
generatePrompt() → getConfig()
↓
buildPrompt(config) → secciones parciales
↓
renderOutput(prompt, config)
↓
Tabs: Preview (HTML) | Raw (texto) | Resumen (cards)
```

---

## 3. Tecnología y Versiones

| Componente | Tecnología | Versión |
|---|---|---|
| **Lenguaje UI** | HTML5 + CSS3 + JavaScript ES6+ | Nativo |
| **Framework UI** | Ninguno (Vanilla JS) | — |
| **Build Tool** | Ninguno | — |
| **Package Manager** | Ninguno | — |
| **Hosting** | Archivo estático / GitHub Pages | — |
| **Control de versiones** | Git + GitHub | — |
| **Repositorio** | github.com/Delmys2020/FuntionalQAPromtGenerate | branch: main |

### Tecnologías QA generadas por la herramienta (no del proyecto en sí)

**Java Stack:**
- Java 21 (LTS), Gradle 8.12 (Kotlin DSL) / Maven 3.9.x
- Cucumber 7.21.0, JUnit 5.11.4, Playwright 1.50.0 / Selenium 4.28.0
- Allure 2.29.0, ExtentReports 5.1.2, Lombok 1.18.36

**Python Stack:**
- Python 3.12+, pytest 8.3.4, pytest-bdd 7.2.0 / Behave 1.2.6
- Playwright 1.50.0 / Selenium 4.28.0, Allure-pytest 2.13.5

---

## 4. Contratos

No existen contratos formales definidos (interfaces, OpenAPI, Pact, etc.) dado que el proyecto no tiene backend ni APIs.

**Contrato implícito del objeto de configuración** (`getConfig()` → `buildPrompt()`):

```javascript
{
  projectName: string,         // obligatorio
  projectDescription: string,
  outputLang: 'es' | 'en',
  language: 'java' | 'python',
  bddFramework: 'cucumber' | 'serenity' | 'behave' | 'pytest-bdd',
  bddVersion: string,
  runner: 'junit5' | 'testng' | 'pytest',
  runnerVersion: string,
  uiTool: 'playwright' | 'selenium' | 'restassured' | 'appium',
  uiToolVersion: string,
  buildTool: 'gradle-kts' | 'maven' | 'pip' | 'poetry',
  appType: 'web' | 'api' | 'mobile-android' | 'mobile-ios' | 'hybrid' | 'hybrid-mobile',
  functionalDeps: string,
  featureBackground: boolean,
  scenarioTypes: Array<'happy'|'negative'|'boundary'|'smoke'|'regression'>,
  maxScenarios: number,
  dataDriven: boolean,
  securityTesting: boolean,
  contractTesting: boolean,
  schemaValidation: boolean,
  accessibility: boolean,
  requirements: string,
  multiEnv: boolean,
  environments: string,
  reporting: 'allure' | 'extent' | 'allure-extent' | 'none',
  cicd: 'github-actions' | 'jenkins' | 'gitlab-ci' | 'none',
  parallelExecution: boolean,
  retryMechanism: boolean,
  headlessMode: boolean,
  testDataMgmt: boolean
}
```

---

## 5. Módulos de Datos

No existe capa de datos persistente. El estado de la aplicación es efímero:

| Variable global | Tipo | Propósito |
|---|---|---|
| `window._lastPrompt` | `string` | Último prompt generado, usado por `copyPrompt()` |
| `window._lastConfig` | `object` | Última configuración usada, disponible para regeneración |

Los datos del formulario residen exclusivamente en el DOM (inputs HTML) y se leen sincrónicamente en cada generación.

---

## 6. Estándares y Convenciones del Código

### Nomenclatura
- **Funciones JS**: camelCase descriptivo — `buildPrompt()`, `getUIToolVersion()`, `markdownToHtml()`
- **IDs del DOM**: kebab-case — `output-section`, `raw-prompt-content`, `bddFramework`
- **Clases CSS**: BEM parcial — `.acc-item`, `.acc-header`, `.tab-btn`, `.summary-card`
- **Variables CSS**: Custom Properties con prefijo semántico — `--bg-primary`, `--text-secondary`, `--accent`

### Patrones aplicados
- **Separación por comentarios de bloque**: cada módulo lógico JS está delimitado con `// ══════ NOMBRE ══════`
- **Template literal strings**: toda la generación de Markdown usa template literals multilínea
- **Condicional ternario compacto**: usado extensivamente para selección de fragmentos de texto según config
- **Guard clauses**: validación de `projectName` al inicio de `generatePrompt()` antes de procesar
- **Fallback de clipboard**: `execCommand('copy')` como fallback cuando la API Clipboard no está disponible

### CSS
- Variables CSS (Custom Properties) para theming completo en `:root`
- Dark theme por defecto inspirado en GitHub Dark
- Responsive con un único breakpoint: `@media(max-width:768px)`
- Animaciones CSS puras (`@keyframes spin` para el spinner del botón)

---

## 7. Riesgos, Supuestos y Limitaciones

### Riesgos
| Riesgo | Impacto | Detalle |
|---|---|---|
| **Archivo único monolítico** | Alto | Todo el proyecto (HTML + CSS + JS) está en un solo archivo de ~1000+ líneas. Dificulta mantenimiento, pruebas y colaboración. |
| **Markdown renderer propio** | Medio | El parser Markdown desarrollado internamente (`markdownToHtml()`) es frágil ante sintaxis compleja o anidada. No soporta listas anidadas, links, imágenes ni escapes. |
| **Sin validación de inputs** | Medio | Solo se valida que `projectName` no esté vacío. Otros campos no tienen validación de formato ni longitud máxima. |
| **Estado global mutable** | Bajo | Uso de `window._lastPrompt` y `window._lastConfig` como estado global expuesto. |
| **Compatibilidad de navegadores** | Bajo | Uso de `navigator.clipboard.writeText()` sin detección de soporte antes de llamar (solo hay try/catch). |

### Supuestos
- El usuario tiene acceso a internet para acceder a GitHub Pages o un servidor HTTP local.
- El usuario conoce el stack tecnológico que desea generar antes de usar la herramienta.
- Los prompts generados se usan en modelos LLM con ventana de contexto suficiente (el prompt puede superar 4000 tokens).
- Las versiones de librerías embebidas en el generador son las estables al año 2026 y deben revisarse periódicamente.

### Limitaciones técnicas
- **Sin persistencia**: el formulario se resetea al recargar la página. No hay guardado de configuraciones.
- **Sin historial**: no se guardan prompts anteriores.
- **Sin descarga ZIP**: a diferencia de lo que se mencionó en descripciones previas, el archivo actual **no incluye** generación de ZIP. Solo genera texto Markdown.
- **Sin internacionalización real**: el campo "idioma de salida" ajusta algunos textos del prompt pero el UI está fijo en español.
- **Sin tests automatizados**: no existe ninguna suite de pruebas para la propia herramienta.

---

## 8. Puntos de Mejora

### Información de entrada — ¿Es suficiente para generar un proyecto de automatización completo?

La herramienta captura una base sólida, pero le faltan los siguientes campos para generar proyectos verdaderamente completos y listos para producción:

### Faltantes de la herramienta
| Campo faltante | Justificación |
|---|---|
| **URL base de la aplicación bajo prueba** | Necesaria para generar configuraciones y hooks reales sin placeholders. |
| **Credenciales de prueba (estructura)** | Definir si se usan variables de entorno, Vault, o archivos cifrados. |
| **Módulos funcionales del sistema** | Actualmente solo genera ejemplos de `login`. Sin lista de módulos, los features son genéricos. |
| **Estrategia de ambientes (URLs por ambiente)** | Se pregunta si hay multi-ambiente pero no se piden las URLs reales. |
| **Tipo de autenticación** | OAuth2, JWT, Basic Auth, SSO — impacta directamente en hooks y steps. |
| **Timeouts personalizados** | Por tipo de acción (carga de página, elemento, API response). |
| **Configuración de reintentos por tipo de falla** | Red, elemento no encontrado, timeout — distintos reintentos. |
| **Gestión de datos de prueba** | Origen de datos: mock, BD de prueba, API seed, Faker — cambia la arquitectura. |
| **Convención de nomenclatura del equipo** | Impacta nombres de clases, métodos y archivos generados. |
| **Versión mínima del navegador objetivo** | Para configurar correctamente Playwright/Selenium. |
| **Política de screenshots** | Solo en falla, siempre, o nunca — impacta hooks y tamaño de reportes. |
| **Integración con gestión de defectos** | Jira, Azure DevOps — para anotaciones en reportes. |
| **Cobertura mínima esperada (%)** | Para configurar umbrales en el pipeline CI/CD. |

### Mejoras técnicas de la herramienta

- **Modularizar el código JS** en archivos separados (módulos ES6 o bundler como Vite).
- **Agregar persistencia** con `localStorage` para no perder configuraciones al recargar.
- **Reemplazar el Markdown renderer** por una librería probada como `marked.js` o `markdown-it`.
- **Agregar validación de formulario** completa con mensajes de error por campo.
- **Implementar tests** para las funciones de generación de prompts (Jest o Vitest).
- **Agregar funcionalidad de descarga ZIP** con la librería `JSZip` para entregar archivos del proyecto directamente.
- **Versionado del esquema de configuración** para permitir compatibilidad futura.
- **Agregar preview de árbol de archivos** antes de la generación completa.

---

> **Nota de mantenimiento:** Este archivo debe actualizarse después de cada implementación, corrección o modificación relevante del proyecto `FuntionalQAPromtGenerate`.
```