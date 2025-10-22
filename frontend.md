# 🌐 Parte 3: Frontend con Angular e Integración con el Backend

## 🎯 Objetivo
Construir una interfaz web moderna con Angular que consuma la API REST creada en el backend, manejando autenticación con Keycloak y mostrando datos de forma dinámica.

---

## 🧩 Temas Clave

### 1. Introducción a Angular
- ¿Qué es Angular y por qué usarlo?
- Arquitectura basada en componentes.
- Instalación y configuración inicial del proyecto con Angular CLI.
- Explicación rápida de los archivos principales (`app.module.ts`, `app.component.ts`, `main.ts`).

---

### 2. Configuración del Entorno
#### Instalación de Node.js y Angular CLI
```bash
node -v
npm -v
npm install -g @angular/cli
ng version
```

#### Creación del proyecto
```bash
ng new frontend-app
cd frontend-app
ng serve
```
Verifica la aplicación en [http://localhost:4200](http://localhost:4200).

---

### 3. Estructura del Proyecto
- **Componentes** → Secciones visuales.
- **Servicios** → Comunicación con el backend.
- **Modelos** → Representación de los datos.
- **Módulos** → Organización del proyecto.

---

### 4. Integración con el Backend
- Uso del módulo `HttpClientModule`.
- Creación de un servicio Angular para consumir la API del backend.
- Configuración de los **endpoints protegidos con Keycloak**.
- Explicación de **CORS** y cómo el backend lo permite.

---

### 5. Autenticación con Keycloak
#### Instalación
```bash
npm install keycloak-js
```

#### Pasos generales
- Crear un archivo `keycloak.service.ts` para inicializar la conexión.
- Configurar `main.ts` para cargar la sesión antes del renderizado.
- Manejar el token JWT desde el frontend.
- Crear un **HTTP Interceptor** para agregar el token automáticamente a cada petición.
- Proteger las rutas de la aplicación con **AuthGuards**.

---

### 6. Visualización de Datos
- Crear un componente de listado (por ejemplo, productos o estudiantes).
- Crear un formulario simple para crear/editar elementos.
- Usar directivas de Angular (`*ngFor`, `*ngIf`) y formularios reactivos (`FormGroup`, `FormControl`).

---

### 7. Pruebas y Despliegue
- Verificar el consumo de API autenticada desde el navegador.
- Generar el build de producción:
```bash
ng build --configuration production
```
- Explicación: el build se puede servir desde **Nginx** o integrarse en un contenedor **Docker**.

---

## ✅ Resultado Esperado
Una aplicación Angular funcional, integrada con el backend y autenticada con Keycloak, capaz de listar y crear datos protegidos mediante token.
