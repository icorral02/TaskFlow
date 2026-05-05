# ✓ TaskFlow

> Aplicación fullstack de gestión de tareas interactvo.

![Node.js](https://img.shields.io/badge/Node.js-Express-green?style=flat-square&logo=node.js)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow?style=flat-square&logo=javascript)
![CSS3](https://img.shields.io/badge/CSS3-Grid%20%26%20Flexbox-blue?style=flat-square&logo=css3)

---

## 📋 Descripción

**TaskFlow** es una aplicación web fullstack que permite gestionar tareas de forma ágil y visual. Cuenta con un backend construido en Node.js utilizando Express que expone un CRUD completo, y un frontend en HTML, CSS y JavaScript que consume dicha API mediante `fetch` con `async/await`.

- Arquitectura cliente-servidor
- Diseño y consumo de APIs REST
- DOM manipulation y eventos en JavaScript
- CSS moderno (Grid, Flexbox, Custom Properties, animaciones)
- Diseño Responsivo

---

## 🚀 Características

- **Crear tareas** con título, descripción, categoría y prioridad
- **Marcar como completadas** o revertirlas a pendiente
- **Eliminar tareas** con confirmación
- **Filtrar tareas** por categoría (Estudio, Práctica, Proyecto, Personal, General)
- **Contadores en tiempo real** de tareas totales y completadas en el header
- **Prioridades visuales**: Alta, Media y Baja con colores diferenciados
- **Validación de formulario** del lado del cliente con mensajes de error
- **Diseño responsivo** adaptado a móvil, tablet y escritorios
- **Animaciones suaves** al renderizar las tarjetas

---

## 🛠️ Stack Tecnológico

| Capa | Tecnología |
|------|-----------|
| **Backend** | Node.js + Express 5 |
| **Frontend** | HTML5 semántico + CSS3 + JavaScript ES6+ |
| **HTTP Client** | Fetch API (async/await) |
| **Estilos** | CSS Grid, Flexbox, Custom Properties |
| **Datos** | Array en memoria (sin base de datos) |

---

## 📁 Estructura del Proyecto

```
taskflow/
│
├── server.js          
├── package.json       
├── package-lock.json  
│
└── public/            
    ├── index.html     
    ├── css/
    │   └── styles.css 
    └── js/
        └── app.js     
```

> **Nota:** Express sirve la carpeta `public/` como archivos estáticos mediante `express.static('public')`.

---

## ⚙️ Instalación y Uso

### Prerrequisitos

- [Node.js](https://nodejs.org/) v18 o superior
- npm (incluido con Node.js)

### Pasos

```bash
# 1. Clona o descarga el repositorio
git clone https://github.com/tu-usuario/taskflow.git
cd taskflow

# 2. Instala las dependencias
npm install

# 3a. Inicia en modo producción
npm start

# 3b. Inicia en modo desarrollo (con hot-reload)
npm run dev
```

Abre tu navegador en: **[http://localhost:3000](http://localhost:3000)**

---

## 🔌 API REST

El servidor expone los siguientes endpoints bajo el prefijo `/api/tareas`:

### Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| `GET` | `/api/tareas` | Obtener todas las tareas |
| `GET` | `/api/tareas?categoria=estudio` | Filtrar tareas por categoría |
| `GET` | `/api/tareas/:id` | Obtener una tarea por ID |
| `POST` | `/api/tareas` | Crear una nueva tarea |
| `PUT` | `/api/tareas/:id` | Actualizar una tarea completa |
| `PATCH` | `/api/tareas/:id/toggle` | Alternar estado completada/pendiente |
| `DELETE` | `/api/tareas/:id` | Eliminar una tarea |

---

### Modelo de datos — Tarea

```json
{
  "id": 1,
  "titulo": "Aprender HTML5 semántico",
  "descripcion": "Estudiar las etiquetas header, nav, main, section...",
  "categoria": "estudio",
  "prioridad": "alta",
  "completada": false,
  "fechaCreacion": "2026-05-01T12:00:00.000Z"
}
```

**Categorías válidas:** `general` | `estudio` | `practica` | `proyecto` | `personal`

**Prioridades válidas:** `baja` | `media` | `alta`

---

### Ejemplos de peticiones

#### Crear una tarea (POST)

```bash
curl -X POST http://localhost:3000/api/tareas \
  -H "Content-Type: application/json" \
  -d '{
    "titulo": "Estudiar Fetch API",
    "descripcion": "Revisar async/await y manejo de errores",
    "categoria": "estudio",
    "prioridad": "alta"
  }'
```

**Respuesta `201 Created`:**
```json
{
  "exito": true,
  "datos": {
    "id": 3,
    "titulo": "Estudiar Fetch API",
    "descripcion": "Revisar async/await y manejo de errores",
    "categoria": "estudio",
    "prioridad": "alta",
    "completada": false,
    "fechaCreacion": "2026-05-01T15:30:00.000Z"
  }
}
```

#### Obtener tareas filtradas por categoría (GET)

```bash
curl http://localhost:3000/api/tareas?categoria=estudio
```

#### Alternar estado completada (PATCH)

```bash
curl -X PATCH http://localhost:3000/api/tareas/1/toggle
```

#### Eliminar una tarea (DELETE)

```bash
curl -X DELETE http://localhost:3000/api/tareas/1
```

---

### Respuestas de error

Todos los endpoints devuelven errores con el mismo formato:

```json
{
  "exito": false,
  "mensaje": "Tarea no encontrada"
}
```

| Código | Situación |
|--------|-----------|
| `400` | Título faltante o inválido al crear tarea |
| `404` | ID no encontrado en GET, PUT, PATCH o DELETE |

---

## 🎨 Frontend

### Arquitectura del cliente

El archivo `app.js` se divide en cuatro secciones claras:

```
app.js
├── Funciones de API (fetch)
│   ├── obtenerTareas(categoria)
│   ├── crearTarea(tarea)
│   ├── toggleTarea(id)
│   └── eliminarTarea(id)
│
├── Funciones de renderizado
│   ├── crearCardHTML(tarea)   
│   └── renderizarTareas()    
│
├── Manejadores de eventos
│   ├── formTarea (submit)    
│   ├── manejarToggle(id)     
│   └── manejarEliminar(id)   
│
└── Filtros
    └── filtrosContenedor (click) → delegación de eventos
```

### Estilos destacados (`styles.css`)

- **CSS Custom Properties** para toda la paleta de colores y espaciados
- **CSS Grid** para el formulario (2 columnas) y la cuadrícula de tarjetas (`auto-fill`)
- **Flexbox** para la navegación, filtros y acciones dentro de cada tarjeta
- **Pseudo-clases** `:focus`, `:hover`, `:active`, `:invalid` para estados interactivos
- **`@keyframes fadeIn`** para animación de entrada en las tarjetas
- **Media queries** en `768px` y `480px` para diseño responsivo

---