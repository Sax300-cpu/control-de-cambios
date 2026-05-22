# 📋 Explicación del Formulario de Control de Cambios

## 🎯 ¿Qué es y por qué se hizo así?

Es un **formulario digital de control de cambios** para un sistema de venta de boletos de bus. El profesor rechazó la versión anterior porque parecía un formulario de papel con firmas manuales. Esta versión simula un **workflow real de desarrollo de software** (Git/GitHub/DevOps) sin necesidad de backend.

---

## 🏗️ Arquitectura General

```
┌─────────────────────────────────────────────────────┐
│  Un solo archivo HTML (sin dependencias externas)   │
│  ┌──────────────────┬──────────────────────────────┐│
│  │ Tab 1: Solicitud│ Tab 2: Reporte del Cambio    ││
│  │ (Usuarios)       │ (Developers/DevOps)          ││
│  └──────────────────┴──────────────────────────────┘│
│  ↓ Validación → JSON download → GitHub API (futuro) │
└─────────────────────────────────────────────────────┘
```

**Por qué un solo archivo:** La instrucción original pedía cero dependencias, sin build steps, que funcione con doble clic. Esto demuestra dominio de vanilla JS y CSS sin depender de frameworks.

---

## 📋 Tab 1: Solicitud de Cambio (Para Usuarios)

### Flujo del usuario:
1. **Selecciona su rol** → Esto dispara la lógica dinámica
2. **Aparece "Tipo de Cambio"** adaptado a su perfil
3. Llena descripción, motivo, prioridad, adjunta evidencia
4. Envía → Se descarga un JSON

### Lógica dinámica de "Tipo de Cambio":

```javascript
// Tres categorías de roles:
ROLES_USUARIO  = ['Pasajero', 'Chofer']           → Opciones simples
ROLES_TECNICO  = ['Developer']                     → Opciones técnicas
ROLES_HIBRIDO  = ['Administrador', 'Oficinista']   → Opciones técnicas con contexto
```

**¿Por qué así?** Un pasajero no entiende "Modificación de BD" ni "queries N+1". Al seleccionar su rol, el formulario le muestra opciones que entiende:
- Pasajero/Chofer → "Reportar un Problema", "Sugerir una Idea", "Mejora Visual"
- Developer/Admin → "Nueva Regla de Negocio", "Bugfix", "Migration", "Optimización"

La sección está **oculta hasta que se elige un rol**, evitando confusión. Al cambiar de rol, se limpia la selección anterior.

---

## 🔧 Tab 2: Reporte del Cambio (Para Developers)

Este es el corazón del feedback del profesor. Reemplaza firmas manuales con **trazabilidad de código real**:

### Pipeline de estados:
```
Propuesto → En Desarrollo → Validado en Sandbox → Aprobado en Code Review → Mergado a Develop → Desplegado en Producción
```

### Campos de trazabilidad Git:
| Campo | Para qué sirve |
|-------|---------------|
| GitHub Issue ID | Vincula con el issue del repo |
| Feature Branch | Nombre de la rama (`feature/SC-XXX-descripcion`) |
| PR Link | Link al Pull Request |
| Commit Hash | Hash del commit específico |
| Sandbox Testing Status | Estado de pruebas automáticas |
| Technical Reviewer | Quién hizo code review |
| Code Owner Approval | Aprobado / Requiere cambios / Rechazado |

### Validación en Sandbox:
6 módulos del sistema con botones ✅/❌:
- Operativa, Ventanilla, Web Client, Base de Datos, Livewire, Tailwind

### Plan de Rollback:
Incluye comandos reales de git y Laravel:
- `git revert <commit-hash>`
- `php artisan migrate:rollback --step=1`

### Aprobación Digital:
Tabla con Developer, Technical Reviewer y Product Owner — cada uno con nombre, estado y fecha. **Cero firmas manuales.**

---

## 💾 JSON Export

Cada formulario genera un JSON descargable con estructura profesional:

**Solicitud:**
```json
{
  "tipo": "solicitud_de_cambio",
  "numero": "SC-20260521-482",
  "solicitante": { "nombre": "...", "rol": "Pasajero" },
  "tipo_cambio": "Reportar un Problema",
  "estado_github": "pendiente_de_envio"
}
```

**Reporte:**
```json
{
  "tipo": "reporte_del_cambio",
  "trazabilidad": {
    "github_issue_id": "#123",
    "feature_branch": "feature/SC-20260521-482-fix-pagos",
    "pr_link": "https://github.com/...",
    "commit_hash": "abc1234",
    "code_owner_approval": "Aprobado"
  },
  "validacion_sandbox": { "operativa": "pass", "webclient": "pass" },
  "aprobacion_digital": { ... }
}
```

El campo `estado_github: "pendiente_de_envio"` está preparado para cuando se active la integración con la API de GitHub (bloque comentado al final del archivo).

---

## 🎨 Diseño

- **Dark theme** (`#0f1117`) — profesional, técnico
- **Space Grotesk + DM Sans** — fuentes modernas de Google Fonts
- **Tarjetas clickeables** en lugar de checkboxes/radios nativos
- **Colores por prioridad** (rojo=urgente, verde=baja)
- **Colores por estado de pipeline** (azul=propuesto, verde=mergado, etc.)
- **Animaciones** fade-in, hover glow, spinner de carga
- **Responsive** — funciona en móvil y desktop

---

## 🔌 Integración GitHub API (Futura)

Al final del archivo hay un bloque comentado con:
- `GITHUB_CONFIG` para token, owner, repo
- `crearIssueGitHub()` para POST a la API
- `formatearSolicitudMarkdown()` y `formatearReporteMarkdown()` para generar el body del issue

Para activarlo: descomentar, llenar el config, y llamar la función al enviar el formulario.

---

## ✅ ¿Cómo responde al feedback del profesor?

| Feedback del profesor | Solución implementada |
|---|---|
| Roles universitarios (Estudiante, Docente) | Reemplazados por roles del sistema: Admin, Oficinista, Chofer, Pasajero, Developer |
| Formulario de papel con firmas | Workflow digital con pipeline de estados, PR links, commit hashes |
| Tipo de cambio genérico | Dinámico según rol: opciones simples para usuarios, técnicas para devs |
| Sin trazabilidad de código | GitHub Issue ID, Feature Branch, PR Link, Commit Hash, Code Owner Approval |
| Sin validación de pruebas | Grid de 6 módulos con estado ✅/❌ en sandbox |
| Origen de solicitud vago | Retroalimentación Usuario, Requerimiento Ingeniero, Falla Crítica, Iniciativa Técnica |

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
