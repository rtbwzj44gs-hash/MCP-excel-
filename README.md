# MCP de Microsoft Excel

**Proyecto individual — Ingeniería Comercial | Aplicación académica en Contabilidad**

Este proyecto implementa un servidor **Model Context Protocol (MCP)** que conecta un asistente de inteligencia artificial, mediante **OpenCode**, con **Microsoft Excel instalado en macOS**. Permite que la IA interactúe con libros y hojas de cálculo mediante herramientas definidas en el servidor, en lugar de limitarse a describir cómo trabajar en una planilla.

El proyecto se desarrolló a partir del `mcp-starter-kit` utilizado en clases y está orientado a apoyar tareas académicas como la organización de registros contables, la preparación de tablas y la comprobación de cálculos. La interpretación y validación contable siguen siendo responsabilidad del estudiante.

## ¿Qué problema resuelve?

En Contabilidad, ingresar datos, organizar asientos y preparar planillas puede consumir tiempo que podría dedicarse a comprender los procedimientos. Este MCP ayuda a trasladar instrucciones y resultados a Excel, facilitando el trabajo operativo y permitiendo concentrarse en el análisis de la información.

**Destinatarios:** estudiantes que utilicen Microsoft Excel para desarrollar ejercicios académicos, especialmente de Contabilidad e Ingeniería Comercial.

## Herramientas disponibles

| Herramienta | Función | Modifica datos |
| --- | --- | --- |
| `estado_excel` | Consulta el estado de Excel, versión y contexto activo. | No |
| `listar_libros` | Lista los libros y hojas disponibles. | No |
| `leer_rango` | Lee valores de un rango, por ejemplo `A1:D20`. | No |
| `abrir_libro` | Abre un archivo compatible en Excel. | No modifica celdas |
| `crear_libro` | Crea un libro nuevo. | Sí |
| `crear_hoja` | Agrega una hoja al libro. | Sí |
| `escribir_rango` | Escribe valores o fórmulas en celdas. Requiere `confirmar: true`. | Sí |
| `guardar_libro` | Guarda el libro en una ruta indicada. Requiere `confirmar: true`. | Sí |

> Las funciones anteriores corresponden al código fuente de la versión revisada del proyecto. Algunas operaciones pueden depender de los permisos de macOS y del estado de Excel.

## Requisitos

- **macOS** con Microsoft Excel instalado (ruta habitual: `/Applications/Microsoft Excel.app`).
- **Node.js 18 o superior** y npm.
- **OpenCode** para interactuar con el servidor MCP desde un asistente de IA.
- Permisos de **Automatización** de macOS para que la aplicación que ejecuta OpenCode pueda controlar Excel.

## Instalación paso a paso

1. Clona el repositorio y entra a su carpeta:

   ```bash
   git clone https://github.com/rtbwzj44gs-hash/MCP-excel-.git
   cd MCP-excel-
   ```

2. Instala las dependencias:

   ```bash
   npm install
   ```

3. Compila el servidor:

   ```bash
   npm run build
   ```

4. Ejecuta la prueba de humo:

   ```bash
   npm test
   ```

   Esta prueba comprueba la compilación y aspectos básicos del protocolo MCP, pero **no sustituye una prueba real de lectura y escritura en Excel**.

5. Configura OpenCode para utilizar el servidor desde la carpeta del proyecto:

   ```bash
   node scripts/configurar-opencode.mjs --nombre=microsoft-excel
   ```

   Revisa el archivo `opencode.json` generado y confirma que apunte al `dist/index.js` de **tu propia instalación**. No copies rutas de usuario de otro computador.

6. Abre Excel y OpenCode. En macOS puedes utilizar el lanzador incluido:

   ```bash
   ./Abrir-microsoft-excel.command
   ```

7. Cuando macOS lo solicite, autoriza el control de Excel en **Ajustes del Sistema → Privacidad y seguridad → Automatización**.

### Verificación práctica

Antes de trabajar con archivos importantes, prueba estas acciones en un libro nuevo:

1. Consultar `estado_excel`.
2. Crear un libro mediante `crear_libro`.
3. Escribir una tabla pequeña con `escribir_rango`, autorizando la operación.
4. Leer la tabla con `leer_rango` y contrastarla con lo que aparece en Excel.
5. Guardar el libro con `guardar_libro`, autorizando la operación.

## Caso de uso académico: PPE y depreciación

Como demostración se utiliza un ejercicio de **Propiedad, Planta y Equipo (PPE)** de Contabilidad: una empresa importa una máquina embotelladora y debe calcular su costo inicial, registrar la adquisición en el libro diario y determinar la depreciación correspondiente al cierre del año.

**Antes del MCP:** el estudiante debía organizar manualmente los antecedentes, ingresar las cuentas y montos en Excel y construir las tablas necesarias.

**Con el MCP:** se entrega el enunciado a la IA; el asistente prepara los datos y utiliza las herramientas del servidor para crear o modificar el libro, registrar los resultados y guardarlos en Excel.

**Después:** se obtiene una planilla con el desarrollo del ejercicio que puede revisarse y corregirse. El estudiante debe comprobar los asientos, el tratamiento del IVA, las retenciones, el costo capitalizable y el período de depreciación aplicado.

### Ejemplo de instrucción

> Resuelve este ejercicio de PPE y organiza el resultado en Microsoft Excel usando exclusivamente las herramientas de mi MCP. Crea hojas para el cálculo del costo, el libro diario y la depreciación. Muestra las operaciones realizadas, verifica los resultados y guarda el libro en un archivo nuevo. No modifiques otros archivos. [Adjuntar el enunciado del ejercicio].

La **demostración en video** debe mostrar el enunciado inicial, las operaciones reales del MCP y el resultado final en Excel. El video se entrega por separado o mediante un enlace accesible.

## Uso de inteligencia artificial

Se utilizaron herramientas de inteligencia artificial como apoyo para la programación, configuración, resolución de problemas técnicos y preparación de la demostración. Las respuestas de la IA no reemplazan la revisión humana: los cálculos y registros contables deben comprobarse antes de considerarse correctos.

## Seguridad y permisos

- Utilizar Excel y archivos propios o expresamente autorizados.
- Conceder únicamente los permisos necesarios para la automatización de Excel.
- Probar las operaciones de escritura en libros nuevos y mantener copias de seguridad.
- Confirmar antes de sobrescribir celdas o guardar archivos; las herramientas `escribir_rango` y `guardar_libro` requieren `confirmar: true`.
- No publicar archivos `.env`, tokens, claves API, datos financieros privados ni rutas personales en el repositorio.
- No subir `node_modules`, registros con datos privados ni archivos temporales.

## Limitaciones conocidas

- La automatización está diseñada para **macOS** y depende de **AppleScript**; no debe asumirse compatibilidad funcional con Windows.
- Los permisos de macOS pueden impedir operaciones sobre Excel (por ejemplo, error `-1743`).
- Los diálogos de Excel, libros protegidos o archivos ocupados pueden interrumpir una operación.
- La lectura de celdas puede depender del formato visible de Excel.
- La prueba `npm test` verifica aspectos técnicos básicos, no garantiza por sí sola que todas las herramientas funcionen sobre la aplicación real.
- Durante una demostración anterior se observaron dificultades en algunas operaciones (`listar_libros`, `crear_hoja` y escritura de fórmulas mediante `escribir_rango`). **Se debe volver a comprobar su estado en la versión publicada y actualizar esta sección según los resultados.**

## Estructura principal del proyecto

```text
src/                  Código fuente del servidor y herramientas
scripts/              Configuración, diagnóstico y pruebas
package.json          Dependencias y comandos npm
package-lock.json     Versiones de dependencias
opencode.json         Configuración local de OpenCode (revisar rutas antes de publicar)
Abrir-microsoft-excel.command  Lanzador para macOS
README.md             Documentación del proyecto
```

La carpeta `dist/` se genera mediante `npm run build`. La carpeta `node_modules/` se crea con `npm install` y no debe publicarse.

## Comandos útiles

```bash
npm install       # Instalar dependencias
npm run build     # Compilar TypeScript
npm test          # Ejecutar prueba de humo
npm run inspect   # Abrir inspector MCP
npm run diagnostico  # Ejecutar diagnóstico del proyecto
```

## Reproducibilidad

Otra persona puede clonar el repositorio, instalar dependencias, compilar, configurar OpenCode y probar el servidor en su propia instalación de Excel. Es necesario que el repositorio contenga **los archivos fuente descomprimidos**, y no solamente un ZIP, y que la configuración no dependa de rutas personales del autor.

---

**Nota de publicación:** esta documentación se preparó a partir de una copia del proyecto facilitada anteriormente. Antes de entregar el examen, confirma que los comandos, nombres de herramientas, permisos y limitaciones coincidan con la versión final que está publicada en GitHub.
