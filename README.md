# Astro + HTMX + Alpine.js Training (24hs)

**Duración total:** 24hs (Astro 8hs, HTMX 8hs, Alpine.js 8hs)

## Objetivo

Producir un manual para desarrolladores que explique paso a paso el funcionamiento de las tecnologías Astro, HTMX y Alpine.js, con enlaces de interés. El objetivo es que el próximo profesional tenga un punto de partida sólido para aprender estas tecnologías y pueda implementar un POC con las siguientes características:

- **Grid que muestre datos**.
- **CRUD sobre el grid**.
- **Validación usando Alpine.js**.
- **Cero vanilla JS** (solo HTMX y Alpine.js).
- **Subirlo a GitHub (Novicell)**.

---

## Índice

1. [Requisitos previos](#requisitos-previos)
2. [Astro (8hs)](#astro-8hs)
3. [HTMX (8hs)](#htmx-8hs)
4. [Alpinejs (8hs)](#alpinejs-8hs)
5. [POC: Grid + CRUD + Validaciones](#poc-grid--crud--validaciones)
6. [Checklist de entrega](#checklist-de-entrega)
7. [Enlaces de interés](#enlaces-de-interés)

---

## Requisitos previos

- Node.js 18+.
- npm/pnpm.
- Git.
- Acceso a GitHub (organización Novicell).

---

## Astro (8hs)

### 1. ¿Qué es Astro?

Astro es un framework para construir sitios web rápidos y orientados al contenido. Su principal ventaja es el **Island Architecture**, donde solo las partes interactivas se hidratan con JavaScript.

- **Conceptos clave**:
  - **Server-first**: renderiza HTML en el servidor.
  - **Island Architecture**: hidrata solo componentes interactivos.
  - **Componentes .astro**: combinan HTML, CSS y JS.
  - **Integraciones**: React, Vue, Svelte, etc.

### 2. Instalación y setup inicial

1. **Crear proyecto**:
   ```bash
   npm create astro@latest
   ```
2. **Entrar al proyecto**:
   ```bash
   cd <nombre-proyecto>
   ```
3. **Iniciar dev server**:
   ```bash
   npm run dev
   ```

### 3. Estructura típica de Astro

```
/astro-project
  /src
    /pages
      index.astro
    /components
      Card.astro
    /layouts
      BaseLayout.astro
```

- `src/pages`: cada archivo es una ruta.
- `src/components`: componentes reutilizables.
- `src/layouts`: layouts compartidos.

### 4. Paso a paso sugerido (8hs)

| Hora | Tema |
|------|------|
| 1-2  | Introducción + instalación |
| 3-4  | Componentes, layouts y páginas |
| 5-6  | Data fetching + SSR |
| 7-8  | Integración con HTMX y Alpine.js |

---

## HTMX (8hs)

### 1. ¿Qué es HTMX?

HTMX permite agregar interactividad con **atributos HTML**, evitando escribir JS manual. Se comunica con el servidor vía AJAX, WebSockets o SSE.

- **Conceptos clave**:
  - `hx-get`, `hx-post`, `hx-put`, `hx-delete`.
  - `hx-target`: dónde se actualiza el HTML.
  - `hx-swap`: cómo se inyecta el HTML.
  - `hx-trigger`: cuándo se dispara.

### 2. Setup básico

```html
<script src="https://unpkg.com/htmx.org@1.9.12"></script>
```

### 3. Ejemplo simple

```html
<button hx-get="/api/data" hx-target="#grid">Cargar Datos</button>
<div id="grid"></div>
```

### 4. Paso a paso sugerido (8hs)

| Hora | Tema |
|------|------|
| 1-2  | Fundamentos y atributos clave |
| 3-4  | CRUD básico con HTMX |
| 5-6  | Integración con Astro |
| 7-8  | Componentes y patrones (partials) |

---

## Alpine.js (8hs)

### 1. ¿Qué es Alpine.js?

Alpine.js es un micro-framework que permite agregar reactividad directamente en el HTML, ideal para validaciones y pequeñas interacciones.

- **Conceptos clave**:
  - `x-data`: estado local.
  - `x-model`: binding.
  - `x-show`: render condicional.
  - `x-bind`: bind dinámico.

### 2. Setup básico

```html
<script defer src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
```

### 3. Ejemplo de validación

```html
<form x-data="{ name: '' }">
  <input x-model="name" required />
  <p x-show="!name">El nombre es obligatorio</p>
</form>
```

### 4. Paso a paso sugerido (8hs)

| Hora | Tema |
|------|------|
| 1-2  | Fundamentos y sintaxis |
| 3-4  | Formularios y validaciones |
| 5-6  | Integración con HTMX |
| 7-8  | Componentes reutilizables |

---

## POC: Grid + CRUD + Validaciones

### Requerimientos

1. **Grid con datos iniciales** (mock o API).
2. **CRUD completo** usando HTMX (sin fetch manual).
3. **Validación de formularios** con Alpine.js.
4. **Sin vanilla JS**.

### Estructura sugerida del POC

```
/src
  /pages
    index.astro
    /api
      items.astro
  /components
    Grid.astro
    ItemRow.astro
    ItemForm.astro
```

### Paso a paso detallado

1. **Crear layout y página principal**
   - `BaseLayout.astro` con los scripts de HTMX y Alpine.js.
   - `index.astro` con el contenedor del grid y el formulario.

2. **Crear el grid de datos**
   - `Grid.astro`: tabla base con `<thead>` y `<tbody id="grid-body">`.
   - `ItemRow.astro`: fila individual que puede ser re-renderizada por HTMX.

3. **Configurar rutas backend (Astro API)**
   - `/src/pages/api/items.astro` para manejar:
     - `GET` → devuelve HTML con las filas del grid.
     - `POST` → crea item y retorna HTML actualizado.
     - `PUT` → actualiza item y retorna la fila actualizada.
     - `DELETE` → elimina item y retorna la tabla sin esa fila.

4. **Agregar HTMX en el frontend**
   - Listar datos:
     ```html
     <div hx-get="/api/items" hx-trigger="load" hx-target="#grid-body" hx-swap="innerHTML"></div>
     ```
   - Crear item:
     ```html
     <form hx-post="/api/items" hx-target="#grid-body" hx-swap="innerHTML">
       <!-- campos -->
     </form>
     ```
   - Editar item:
     ```html
     <form hx-put="/api/items" hx-target="#row-{{id}}" hx-swap="outerHTML">
       <!-- campos -->
     </form>
     ```
   - Eliminar item:
     ```html
     <button hx-delete="/api/items?id={{id}}" hx-target="#row-{{id}}" hx-swap="outerHTML">
       Eliminar
     </button>
     ```

5. **Validaciones con Alpine.js**
   - Campos requeridos con `x-model`.
   - Mensajes de error con `x-show`.
   - Ejemplo:
     ```html
     <form x-data="{ name: '', error: false }" @submit.prevent="error = !name">
       <input x-model="name" required />
       <p x-show="error">El nombre es obligatorio</p>
       <button type="submit">Guardar</button>
     </form>
     ```

6. **Validar que no se use vanilla JS**
   - Todo el comportamiento dinámico debe estar en HTMX o Alpine.js.

### Criterios de aceptación

- La página principal carga un grid con datos iniciales.
- El grid permite crear, editar y borrar filas sin recargar la página.
- Los formularios muestran validaciones en tiempo real con Alpine.js.
- No se usa `fetch`, `addEventListener` ni otros scripts manuales.

---

## Checklist de entrega

- [ ] Manual documentado en README.
- [ ] POC funcional con grid y CRUD.
- [ ] Validación con Alpine.js.
- [ ] Sin vanilla JS.
- [ ] Subido a GitHub (repo Novicell).

### Subir a GitHub (Novicell)

1. Inicializar git:
   ```bash
   git init
   git add .
   git commit -m "Initial POC"
   ```
2. Crear repo en GitHub (Novicell).
3. Vincular remoto:
   ```bash
   git remote add origin git@github.com:Novicell/<repo>.git
   git branch -M main
   git push -u origin main
   ```

---

## Enlaces de interés

### Astro
- [Documentación oficial](https://docs.astro.build/)
- [Guía de Island Architecture](https://docs.astro.build/en/concepts/islands/)
- [Tutorial oficial Astro](https://docs.astro.build/en/tutorial/)

### HTMX
- [Sitio oficial](https://htmx.org/)
- [Documentación](https://htmx.org/docs/)
- [Ejemplos](https://htmx.org/examples/)

### Alpine.js
- [Documentación oficial](https://alpinejs.dev/)
- [Guía de instalación](https://alpinejs.dev/start-here)
- [Referencia completa](https://alpinejs.dev/directives)
