# 🚀 Guía de Instalación y Entorno — Java Spring Boot + Angular + Docker + Keycloak + PostgreSQL

> Documento para preparar el entorno de desarrollo y la clase práctica "SmartTasks — Spring Boot + Angular + IA + Keycloak".  
> Incluye pasos detallados, variables de entorno y comandos de verificación para Windows (también aplicable en Linux/macOS con pequeñas diferencias).

---

## 📋 Contenido
- Requisitos previos
- Instalación paso a paso (Java, Maven, Node, Angular CLI, Docker, WSL, PostgreSQL, Keycloak)
- Variables de entorno críticas (incluye `M2_HOME` / `JAVA_HOME`)
- Comandos de verificación

---

## 🧰 Requisitos Previos
Asegúrate de tener permisos de **Administrador** en Windows para los pasos que lo requieran (WSL, instalación del kernel, servicios).

---

## ☕ 1. Java JDK 21 (Temurin / Adoptium)
**Descarga:** https://adoptium.net

### Pasos
1. Descarga e instala Temurin JDK 21.
2. Durante la instalación, marca la opción `Set JAVA_HOME` si está disponible.
3. Si no, crea manualmente la variable `JAVA_HOME`.

### Variables (ejemplo)
- `JAVA_HOME` = `C:\Program Files\Eclipse Adoptium\jdk-21`
- Añadir al `Path`: `%JAVA_HOME%\bin`

### Verificar
```bash
java -version
javac -version
```

---

## ⚙️ 2. Apache Maven
**Descarga:** https://maven.apache.org/download.cgi

### Pasos
1. Descarga el zip binario y descomprime en (opcional) `C:\Program Files\Apache\Maven\apache-maven-3.9.x`.
2. Agrega variables de entorno:

- `M2_HOME` = `C:\Program Files\Apache\Maven\apache-maven-3.9.x`
- Añadir al `Path`: `%M2_HOME%\bin`

### Verificar
```bash
mvn -version
```

---

## 🧑‍💻 3. IDE (Spring Tools Suite / IntelliJ / VS Code / Cursor )
- **Spring Tools Suite (STS):** https://spring.io/tools  
- **IntelliJ IDEA:** https://www.jetbrains.com/idea/download  
- **VS Code + extensiones (Java, Spring Boot):** opcional
- **Cursor:** https://cursor.com/download
  
---

## 🧱 4. WSL 2 (Windows Subsystem for Linux) — **Requiere Admin**
Abre PowerShell **como administrador**:

```powershell
wsl --install
# (reinicia si se solicita)
wsl --set-default-version 2
wsl --status
```

Si ya tienes WSL pero necesitas actualizar el kernel:
```powershell
wsl --update
wsl --shutdown
```

---

## 🐋 5. Docker Desktop (Windows)
**Descarga:** https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe

### Requisitos Windows
- Requiere **WSL 2** o Hyper-V. Recomendado: **WSL 2**.

### Pasos
1. Instala Docker Desktop.
2. En Settings → General, habilita:
   - *Use the WSL 2 based engine*
   - *Start Docker Desktop when you log in*
3. En Settings → Resources → WSL Integration, activa tu distro (ej. `Ubuntu`).

### Verificar
```bash
docker --version
docker compose version
docker run hello-world
```


---

## 🐘 6. PostgreSQL (opcional local o via Docker)
**Usar Docker (recomendado):**
```bash
docker run --name postgres -e POSTGRES_PASSWORD=admin -p 5432:5432 -d postgres
```

Verificar:
```bash
docker ps
# o localmente
psql --version
```

---

## 🔐 7. Keycloak (en Docker)
Inicia Keycloak (modo dev):

En cualquier aplicación —ya sea web, móvil o API— tenemos dos necesidades básicas:

Autenticar (¿quién eres tú?)

Autorizar (¿qué puedes hacer?)

```bash
docker run -d --name keycloak -p 8081:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:24.0 start-dev
```

Admin Console: http://localhost:8081  
User: `admin` / Password: `admin`

---

## 🌐 8. Node.js & Angular CLI
**Node.js:** https://nodejs.org

Instala Angular CLI globalmente:
```bash
npm install -g @angular/cli
```

Verificar:
```bash
node -v
npm -v
ng version
```

---

## 🐼 9. Dbeaver (Windows)
**Descarga:** [https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe](https://dbeaver.io/download)

## ⚙️ Requisitos previos
Antes de instalar DBeaver asegúrate de tener instalado:
- **Java 17 o superior** (ya instalado previamente para el backend con Spring Boot).
- **PostgreSQL** (ya configurado en Docker o localmente).

---

## 🔧 Variables de Entorno Importantes (Windows)
Configura estas variables en **Sistema → Configuración avanzada → Variables de entorno**:

- `JAVA_HOME` = `C:\Program Files\Eclipse Adoptium\jdk-21`
-  `M2_HOME` = `C:\Program Files\Apache\Maven\apache-maven-3.9.x`
- `PATH` añadir:
  - `%JAVA_HOME%\bin`
  - `%M2_HOME%\bin` (si usas esta variable)
- (Opcional) `NODE_HOME` si lo deseas

---

## ✅ Comandos de Verificación Rápida
Ejecuta estos comandos en CMD, PowerShell o terminal WSL:

```bash
java -version
javac -version
mvn -version
node -v
npm -v
ng version
docker --version
docker compose version
wsl --status
```

Si todos responden correctamente, tu entorno está listo.

---

## 🛠 Troubleshooting común: `docker run hello-world` da error en Windows

Error típico:
```
docker: error during connect: Head "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/_ping": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
```

### Posibles soluciones
1. **Docker Desktop no está corriendo** — abre Docker Desktop y espera que diga *Docker Engine running*.
2. **WSL no instalado o no está en versión 2** — ejecuta `wsl --status` en PowerShell (Admin) y si hace falta: `wsl --install`.
3. **Reinicia servicios**:
   ```powershell
   wsl --shutdown
   net stop com.docker.service
   net start com.docker.service
   ```
4. **Verifica integración WSL** en Docker Desktop Settings → Resources → WSL Integration → activa tu distro.
5. **Si persiste**, reinicia el equipo y abre Docker Desktop como administrador la primera vez.

---

## 📎 Recursos
- Docker Desktop: https://www.docker.com/products/docker-desktop  
- Keycloak docs: https://www.keycloak.org/documentation  
- Adoptium Temurin JDK: https://adoptium.net  
- Maven: https://maven.apache.org
