# MCP Server for Grep

[![npm version](https://badge.fury.io/js/@247arjun%2Fmcp-grep.svg)](https://badge.fury.io/js/@247arjun%2Fmcp-grep)
[![npm downloads](https://img.shields.io/npm/dm/@247arjun/mcp-grep.svg)](https://www.npmjs.com/package/@247arjun/mcp-grep)

Un servidor del Model Context Protocol (MCP) que proporciona potentes capacidades de búsqueda de texto utilizando la utilidad de línea de comandos `grep`. Este servidor le permite buscar patrones en archivos y directorios utilizando tanto descripciones en lenguaje natural como patrones de regex directos.

<a href="https://glama.ai/mcp/servers/@247arjun/mcp-grep">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@247arjun/mcp-grep/badge" alt="Grep Server MCP server" />
</a>

## Características

### 🧠 Búsqueda en Lenguaje Natural
- Describa lo que está buscando en inglés sencillo
- Conversión automática a patrones de regex apropiados
- Patrones integrados para búsquedas comunes (correos electrónicos, URLs, números de teléfono, etc.)

### 🔍 Capacidades de Búsqueda Avanzadas
- Coincidencia directa de patrones regex
- Búsqueda recursiva de directorios
- Filtrado por extensión de archivo
- Búsqueda sensible/insensible a mayúsculas y minúsculas
- Coincidencia de palabra completa
- Visualización de líneas de contexto
- Recuento de coincidencias
- Listado de archivos con coincidencias

### 🛡️ Seguridad Ante Todo
- Ejecución segura de comandos mediante `child_process.spawn`
- Validación de entrada con esquemas Zod
- Sin vulnerabilidades de inyección de shell
- Validación y saneamiento de rutas

## Instalación

### Método 1: Instalación via NPM (Recomendado)

```bash
# Instalación global
npm install -g @247arjun/mcp-grep

# O instalación local en su proyecto
npm install @247arjun/mcp-grep
```

### Método 2: Desde el Código Fuente

```bash
# Clonar el repositorio
git clone https://github.com/247arjun/mcp-grep.git
cd mcp-grep

# Instalar dependencias
npm install

# Compilar el proyecto
npm run build

# Opcional: Vincular globalmente
npm link
```

### Método 3: Directamente desde GitHub

```bash
# Instalar directamente desde GitHub
npm install -g git+https://github.com/247arjun/mcp-grep.git
```

## Configuración

### Configuración de Claude Desktop

Añada lo siguiente a su archivo de configuración de Claude Desktop:

**Ubicación:**
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%/Claude/claude_desktop_config.json`

**Configuración:**
```json
{
  "mcpServers": {
    "mcp-grep": {
      "command": "mcp-grep",
      "args": []
    }
  }
}
```

**Alternativa: Usando npx (no requiere instalación global)**
```json
{
  "mcpServers": {
    "mcp-grep": {
      "command": "npx",
      "args": ["@247arjun/mcp-grep"]
    }
  }
}
```

**Configuración para Desarrollo Local**
```json
{
  "mcpServers": {
    "mcp-grep": {
      "command": "node",
      "args": ["/absolute/path/to/mcp-grep/build/index.js"]
    }
  }
}
```

Después de añadir la configuración, reinicie Claude Desktop para cargar el servidor MCP.

## Verificación

Pruebe que el servidor esté funcionando:

```bash
# Probar el servidor compilado
node build/index.js

# Debería mostrar: "Grep MCP Server running on stdio"
# Presione Ctrl+C para salir
```

## Herramientas Disponibles

### 1. `grep_search_intent`
Busca utilizando descripciones en lenguaje natural.

**Parámetros:**
- `intent` (string): Descripción en inglés sencillo (ej. "email addresses", "TODO comments")
- `target` (string): Ruta del archivo o directorio a buscar
- `case_sensitive` (boolean, opcional): Búsqueda sensible a mayúsculas (predeterminado: false)
- `max_results` (number, opcional): Límite del número de resultados
- `show_context` (boolean, opcional): Mostrar líneas circundantes (predeterminado: false)
- `context_lines` (number, opcional): Número de líneas de contexto (predeterminado: 2)

**Ejemplo:**
```javascript
{
  "intent": "email addresses",
  "target": "./src",
  "show_context": true,
  "context_lines": 1
}
```

### 2. `grep_regex`
Busca utilizando patrones regex directos.

**Parámetros:**
- `pattern` (string): Patrón de expresión regular
- `target` (string): Ruta del archivo o directorio a buscar
- `case_sensitive` (boolean, opcional): Búsqueda sensible a mayúsculas
- `whole_words` (boolean, opcional): Coincidir solo con palabras completas
- `invert_match` (boolean, opcional): Mostrar líneas que no coinciden
- `max_results` (number, opcional): Límite de resultados
- `show_context` (boolean, opcional): Mostrar líneas de contexto
- `context_lines` (number, opcional): Recuento de líneas de contexto
- `file_extensions` (array, opcional): Filtrar por extensiones de archivo

**Ejemplo:**
```javascript
{
  "pattern": "function\\s+\\w+\\s*\\(",
  "target": "./src",
  "file_extensions": ["js", "ts"],
  "show_context": true
}
```

### 3. `grep_count`
Cuenta las coincidencias de un patrón.

**Parámetros:**
- `pattern` (string): Patrón a contar
- `target` (string): Objetivo de búsqueda
- `case_sensitive` (boolean, opcional): Sensibilidad a mayúsculas
- `whole_words` (boolean, opcional): Coincidencia de palabra completa
- `by_file` (boolean, opcional): Mostrar recuento por archivo
- `file_extensions` (array, opcional): Filtro de extensión de archivo

### 4. `grep_files_with_matches`
Lista los archivos que contienen el patrón.

**Parámetros:**
- `pattern` (string): Patrón de búsqueda
- `target` (string): Directorio a buscar
- `case_sensitive` (boolean, opcional): Sensibilidad a mayúsculas
- `whole_words` (boolean, opcional): Coincidencia de palabra completa
- `file_extensions` (array, opcional): Extensiones de archivo a incluir
- `exclude_patterns` (array, opcional): Patrones de archivo a excluir

### 5. `grep_advanced`
Ejecuta grep con argumentos personalizados (usuarios avanzados).

**Parámetros:**
- `args` (array): Matriz de argumentos de grep (excluyendo 'grep' en sí mismo)

## Patrones de Lenguaje Natural Integrados

El servidor reconoce estas intenciones de lenguaje natural:

### Comunicación
- "email", "email address", "emails" → Patrón de dirección de correo electrónico
- "url", "urls", "website", "link", "links" → Patrón de URL
- "phone", "phone number", "phone numbers" → Patrón de número de teléfono

### Red
- "ip", "ip address", "ip addresses" → Patrón de dirección IPv4

### Tipos de Datos
- "number", "numbers", "integer", "integers" → Patrones numéricos
- "date", "dates" → Patrones de fecha

### Patrones de Código
- "function", "functions" → Declaraciones de función
- "class", "classes" → Definiciones de clase
- "import", "imports" → Sentencias de importación
- "export", "exports" → Sentencias de exportación
- "comment", "comments" → Líneas de comentario
- "todo", "todos" → Comentarios TODO/FIXME/HACK

### Patrones de Error
- "error", "errors" → Mensajes de error
- "warning", "warnings" → Mensajes de advertencia

## Ejemplos de Uso

### Buscar direcciones de correo electrónico en un proyecto
```javascript
{
  "tool": "grep_search_intent",
  "intent": "email addresses",
  "target": "./src",
  "show_context": true
}
```

### Encontrar todos los comentarios TODO
```javascript
{
  "tool": "grep_search_intent", 
  "intent": "todo comments",
  "target": "./",
  "file_extensions": ["js", "ts", "py"]
}
```

### Buscar definiciones de funciones con regex
```javascript
{
  "tool": "grep_regex",
  "pattern": "^\\s*function\\s+\\w+",
  "target": "./src",
  "file_extensions": ["js"]
}
```

### Contar ocurrencias de una palabra
```javascript
{
  "tool": "grep_count",
  "pattern": "async",
  "target": "./src",
  "by_file": true
}
```

### Listar archivos que contienen sentencias de importación
```javascript
{
  "tool": "grep_files_with_matches",
  "pattern": "^import",
  "target": "./src",
  "file_extensions": ["js", "ts"]
}
```

## Desarrollo

### Compilar y Ejecutar
```bash
# Desarrollo con auto-reconstrucción
npm run dev

# Compilación de producción
npm run build

# Iniciar el servidor
npm start
```

### Estructura del Proyecto
```
mcp-grep/
├── src/
│   └── index.ts          # Implementación principal del servidor
├── build/                # Salida de JavaScript compilado
├── package.json          # Configuración del proyecto
├── tsconfig.json         # Configuración de TypeScript
└── README.md            # Este archivo
```

## Solución de Problemas

### Problemas Comunes

1. **Error "Command not found"**
   - Asegúrese de que mcp-grep esté instalado globalmente: `npm install -g @247arjun/mcp-grep`
   - O use npx: `"command": "npx", "args": ["@247arjun/mcp-grep"]`

2. **Error "Permission denied"**
   - Verifique los permisos del archivo: `chmod +x build/index.js`
   - Reconstruya el proyecto: `npm run build`

3. **El servidor MCP no aparece en Claude**
   - Verifique la sintaxis JSON en el archivo de configuración
   - Reinicie Claude Desktop completamente
   - Verifique que la ruta del comando sea correcta

4. **"grep command not found"**
   - Instale grep en su sistema (generalmente preinstalado en macOS/Linux)
   - Usuarios de Windows: Instale a través de WSL o use Git Bash

### Depuración

Habilite el registro detallado configurando la variable de entorno:
```bash
# Para desarrollo
DEBUG=1 node build/index.js

# Probar con una entrada de muestra
echo '{"jsonrpc": "2.0", "method": "initialize", "params": {}}' | node build/index.js
```

## Notas de Seguridad

- Utiliza `spawn` con `shell: false` para prevenir la inyección de comandos
- Valida todas las rutas de archivos antes de la ejecución
- Bloquea banderas de grep potencialmente peligrosas en el modo avanzado
- Validación de entrada con esquemas Zod
- Sin acceso a archivos del sistema fuera de los objetivos especificados
