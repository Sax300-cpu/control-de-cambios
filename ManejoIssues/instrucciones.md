# 📋 Instrucciones: App de Control de Cambios

> Documento para agente de IA. Leer completamente antes de escribir código.

---

## 🎯 Objetivo

Construir una aplicación web **local** (un solo archivo HTML) para gestión de control de cambios de software, con dos formularios diferenciados por rol:

1. **Solicitud de Cambio** → Para usuarios finales (sin cuenta GitHub)
2. **Reporte del Cambio** → Para desarrolladores internos

Al enviar, los datos se **guardan como JSON** (descarga automática al navegador). La arquitectura debe estar preparada para conectar con la **API de GitHub** más adelante (crear Issues) con solo agregar un token.

---

## 🗂️ Entregable

- **Un solo archivo:** `index.html`
- **Sin dependencias externas instalables** (CDN está permitido)
- **Sin backend** por ahora — todo corre en el navegador
- **Funciona abriendo el archivo directamente** con doble clic o `file://`

---

## 🎨 Diseño y Estética

### Dirección visual
- Tema: **dark/industrial-refinado** — profesional, moderno, técnico
- NO usar colores rojos (las imágenes de referencia los usan, pero el cliente quiere algo diferente)
- Paleta sugerida:
  - Fondo principal: `#0f1117`
  - Superficie de cards: `#1a1d27`
  - Borde sutil: `#2a2d3e`
  - Acento primario: `#6c63ff` (violeta eléctrico) o `#00d4aa` (cyan/teal)
  - Texto principal: `#e8eaf0`
  - Texto secundario: `#8b8fa8`
  - Estado éxito: `#22c55e`
  - Estado error: `#ef4444`

### Tipografía
- Usar Google Fonts CDN
- Display/títulos: `'Syne'` o `'Space Grotesk'` — fuerte y técnico
- Cuerpo: `'DM Sans'` o `'Outfit'` — limpio y legible
- **NUNCA** Arial, Roboto, Inter

### Layout
- Navegación superior con dos tabs claramente diferenciados
- Formularios centrados, max-width ~760px
- Secciones con headers de color acento (reemplazando los rojos del diseño original)
- Checkboxes estilizados como tarjetas seleccionables (no checkboxes nativos)
- Campos de texto con borde que brilla al focus
- Botón de envío prominente con animación hover

### Animaciones
- Fade-in suave al cargar la página
- Transición fluida al cambiar entre tabs
- Feedback visual al enviar (spinner breve → mensaje de éxito)
- Hover en campos con transición de borde/glow

---

## 📝 Formulario 1: Solicitud de Cambio (Tab para Usuarios)

### Header del formulario
- Ícono de usuario + campo "Nombre del Solicitante" (texto libre)
- Campo "Fecha" (date picker, pre-relleno con fecha actual)
- Número de solicitud (auto-generado: `SC-YYYYMMDD-XXX`, ej: `SC-20250521-001`)

### Secciones

#### ROL DEL SOLICITANTE
Selección única (tarjetas clickeables):
- [ ] Estudiante
- [ ] Docente  
- [ ] Público General

#### DETALLES DEL CAMBIO
- **Descripción del Cambio** — textarea grande (mínimo 4 líneas)
- **Motivo del Cambio** — textarea grande (mínimo 4 líneas)

#### PRIORIDAD DEL CAMBIO
Selección única (tarjetas con color indicativo):
- [ ] Urgente 🔴
- [ ] Alta 🟠
- [ ] Media 🟡
- [ ] Baja 🟢

#### TIPO DE CAMBIO
Selección múltiple (checkboxes estilizados):
- [ ] Permisos o acceso
- [ ] Gráfico
- [ ] Configuraciones
- [ ] Seguridad y privacidad
- [ ] Técnicos
- [ ] Otros

#### ORIGEN DE LA SOLICITUD
Selección única:
- [ ] Fallas
- [ ] Solicitud de mejora

#### DOCUMENTOS O INFORMACIÓN ADICIONAL
- Botón "Adjuntar archivo o imagen" (input file, acepta imágenes y PDFs)
- Mostrar nombre del archivo adjuntado (no se sube a ningún server, solo se registra el nombre)

### Botón de envío
```
[ ENVIAR SOLICITUD ]
```
Al hacer clic:
1. Validar campos obligatorios (Nombre, Descripción, Motivo, Prioridad, Rol)
2. Si hay error: mostrar mensaje rojo debajo del campo faltante
3. Si OK: generar JSON → descargar automáticamente → mostrar modal de éxito

---

## 📝 Formulario 2: Reporte del Cambio (Tab para Desarrolladores)

### Header del formulario
- Número de reporte (auto-generado: `RC-YYYYMMDD-XXX`)

### Secciones

#### DESCRIPCIÓN DEL CAMBIO
- **Descripción del Cambio** — input de una línea
- **Detalle del Cambio** — textarea grande (mínimo 5 líneas)

#### ESTADO
Selección única (tarjetas con indicadores de color):
- [ ] Pendiente
- [ ] En Desarrollo
- [ ] Finalizado

#### DETALLE DE REQUERIMIENTO

**PRIORIDAD** (selección única, tarjetas):
- [ ] Alta
- [ ] Media
- [ ] Baja
- [ ] Urgente

**JUSTIFICACIÓN**:
- textarea mediana (mínimo 4 líneas)

#### IMPACTO DEL CAMBIO
Campos en layout de dos columnas:
- Alcance (input, ancho completo)
- Costo (input) | Entregas (input)
- Partes Interesadas (input) | Recursos (input)

#### ANÁLISIS DE RIESGOS
3 textareas apiladas (para ingresar hasta 3 riesgos identificados)

#### PLAN DE CONTINGENCIA
3 textareas apiladas (para ingresar hasta 3 planes de contingencia)

#### AUTORIZA POR RESPONSABLE
- Nombre (input)
- Cargo (input)
- Firma (input de texto, valor simbólico)

#### APROBADO POR GERENTE
- Nombre (input)
- Cargo (input)
- Firma (input de texto, valor simbólico)

### Botón de envío
```
[ GUARDAR REPORTE ]
```
Misma lógica de validación y descarga JSON que el formulario 1.

---

## 💾 Estructura del JSON generado

### Solicitud de Cambio (`solicitud-SC-YYYYMMDD-XXX.json`)
```json
{
  "tipo": "solicitud_de_cambio",
  "numero": "SC-20250521-001",
  "fecha_creacion": "2025-05-21T10:30:00",
  "solicitante": {
    "nombre": "Carol Cañizares",
    "rol": "Docente"
  },
  "detalles": {
    "descripcion": "...",
    "motivo": "..."
  },
  "prioridad": "Alta",
  "tipo_cambio": ["Gráfico", "Configuraciones"],
  "origen": "Solicitud de mejora",
  "archivo_adjunto": "diagrama.png",
  "estado_github": "pendiente_de_envio"
}
```

### Reporte del Cambio (`reporte-RC-YYYYMMDD-XXX.json`)
```json
{
  "tipo": "reporte_del_cambio",
  "numero": "RC-20250521-001",
  "fecha_creacion": "2025-05-21T10:30:00",
  "descripcion": "...",
  "detalle": "...",
  "estado": "En Desarrollo",
  "requerimiento": {
    "prioridad": "Urgente",
    "justificacion": "..."
  },
  "impacto": {
    "alcance": "...",
    "costo": "...",
    "entregas": "...",
    "partes_interesadas": "...",
    "recursos": "..."
  },
  "analisis_riesgos": ["Riesgo 1", "Riesgo 2", "Riesgo 3"],
  "plan_contingencia": ["Plan 1", "Plan 2", "Plan 3"],
  "autoriza": {
    "nombre": "...",
    "cargo": "...",
    "firma": "..."
  },
  "aprueba": {
    "nombre": "...",
    "cargo": "...",
    "firma": "..."
  },
  "estado_github": "pendiente_de_envio"
}
```

---

## 🔌 Estructura preparada para GitHub API (NO implementar aún)

Agregar este bloque comentado al final del HTML para uso futuro:

```javascript
/* 
=== INTEGRACIÓN GITHUB API (FUTURA) ===

Para activar, descomentar y completar:

const GITHUB_CONFIG = {
  token: 'ghp_XXXXXXXXXXXXXXXXXXXX',  // Personal Access Token
  owner: 'nombre-usuario',            // Usuario u organización
  repo: 'nombre-repositorio',         // Repositorio destino
  labels: {
    solicitud: ['solicitud-cambio', 'needs-review'],
    reporte: ['reporte-cambio', 'dev-task']
  }
};

async function crearIssueGitHub(datos) {
  const esReporte = datos.tipo === 'reporte_del_cambio';
  
  const body = esReporte
    ? formatearReporteMarkdown(datos)
    : formatearSolicitudMarkdown(datos);

  const response = await fetch(
    `https://api.github.com/repos/${GITHUB_CONFIG.owner}/${GITHUB_CONFIG.repo}/issues`,
    {
      method: 'POST',
      headers: {
        'Authorization': `token ${GITHUB_CONFIG.token}`,
        'Content-Type': 'application/json',
        'Accept': 'application/vnd.github.v3+json'
      },
      body: JSON.stringify({
        title: `[${datos.numero}] ${datos.descripcion || datos.detalles?.descripcion}`,
        body: body,
        labels: esReporte
          ? GITHUB_CONFIG.labels.reporte
          : GITHUB_CONFIG.labels.solicitud
      })
    }
  );

  if (!response.ok) throw new Error('Error al crear Issue en GitHub');
  return await response.json();
}

function formatearSolicitudMarkdown(d) {
  return `
## 📋 Solicitud de Cambio ${d.numero}
**Solicitante:** ${d.solicitante.nombre} (${d.solicitante.rol})  
**Fecha:** ${d.fecha_creacion}  
**Prioridad:** ${d.prioridad}  
**Origen:** ${d.origen}

### Descripción
${d.detalles.descripcion}

### Motivo
${d.detalles.motivo}

### Tipo de Cambio
${d.tipo_cambio.map(t => '- ' + t).join('\n')}
  `.trim();
}

function formatearReporteMarkdown(d) {
  return `
## 🔧 Reporte del Cambio ${d.numero}
**Estado:** ${d.estado}  
**Prioridad:** ${d.requerimiento.prioridad}  
**Fecha:** ${d.fecha_creacion}

### Descripción
${d.descripcion}

### Detalle
${d.detalle}

### Justificación
${d.requerimiento.justificacion}

### Impacto
| Campo | Valor |
|-------|-------|
| Alcance | ${d.impacto.alcance} |
| Costo | ${d.impacto.costo} |
| Entregas | ${d.impacto.entregas} |
| Partes Interesadas | ${d.impacto.partes_interesadas} |
| Recursos | ${d.impacto.recursos} |

### Análisis de Riesgos
${d.analisis_riesgos.filter(Boolean).map((r, i) => `${i+1}. ${r}`).join('\n')}

### Plan de Contingencia
${d.plan_contingencia.filter(Boolean).map((p, i) => `${i+1}. ${p}`).join('\n')}

### Firmas
| Rol | Nombre | Cargo |
|-----|--------|-------|
| Responsable | ${d.autoriza.nombre} | ${d.autoriza.cargo} |
| Gerente | ${d.aprueba.nombre} | ${d.aprueba.cargo} |
  `.trim();
}
*/
```

---

## ✅ Checklist de Implementación

El agente debe verificar que el entregable cumpla todo esto:

### Funcionalidad
- [ ] Dos tabs funcionales (Solicitud / Reporte) con transición animada
- [ ] Todos los campos del Formulario 1 presentes y funcionales
- [ ] Todos los campos del Formulario 2 presentes y funcionales
- [ ] Validación de campos obligatorios con mensajes de error
- [ ] Auto-generación de número de solicitud/reporte
- [ ] Fecha actual pre-rellenada
- [ ] Descarga automática de JSON al enviar
- [ ] Modal de confirmación tras envío exitoso
- [ ] Botón de "Nueva Solicitud / Nuevo Reporte" que resetea el formulario

### Diseño
- [ ] Tema oscuro profesional (no usar rojo como color principal)
- [ ] Fuentes de Google Fonts (no Arial, no Inter)
- [ ] Checkboxes/radios reemplazados por tarjetas clickeables
- [ ] Focus visible y estilizado en todos los inputs
- [ ] Diseño responsive (funciona en pantalla de laptop y tablet)
- [ ] Headers de sección con color acento (no rojo)

### Código
- [ ] Todo en un solo `index.html`
- [ ] Sin dependencias npm ni build steps
- [ ] Comentario con bloque de GitHub API para integración futura
- [ ] Código limpio y comentado en español
- [ ] JSON con campo `estado_github: "pendiente_de_envio"` para tracking futuro

---

## 🚀 Cómo ejecutar

```bash
# Opción 1: Doble clic en index.html (abre en navegador)

# Opción 2: Servidor local rápido (si tiene Python)
python -m http.server 8080
# Abrir: http://localhost:8080

# Opción 3: Con Node.js
npx serve .
# Abrir: http://localhost:3000
```

---

## 📌 Notas importantes

1. **No hay backend** — toda la lógica es client-side JavaScript
2. **El archivo JSON se descarga** al navegador del usuario al enviar el formulario
3. **El campo `estado_github`** en el JSON permitirá identificar qué registros aún no se han enviado a GitHub cuando se active la integración
4. **La integración con GitHub** requiere solo: descomentar el bloque de código y llenar `GITHUB_CONFIG` con el token y datos del repositorio
5. **El token de GitHub** debe ser un *Personal Access Token* con scope `repo` para poder crear Issues

---

*Documento generado para presentación — versión local sin backend*  
*Próxima fase: Integración con GitHub Issues API*
