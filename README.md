# 📚 Biblioteca DevOps

Proyecto desarrollado para la asignatura **Ingeniería DevOps**.

El sistema corresponde a una aplicación de biblioteca construida utilizando una arquitectura de microservicios con **Spring Boot**, **Spring Cloud**, **Netflix Eureka**, **API Gateway**, control de versiones con **Git/GitHub**, integración continua con **GitHub Actions** y despliegue en una instancia **AWS EC2**.

---

## 📌 Descripción del proyecto

Biblioteca DevOps es una aplicación distribuida compuesta por distintos microservicios.

Cada servicio cumple una función específica dentro del sistema y se comunica mediante una arquitectura basada en Spring Cloud.

Para el descubrimiento de servicios se utiliza **Netflix Eureka**, mientras que el **API Gateway** funciona como punto de entrada principal hacia los distintos microservicios.

---

# 🏗️ Arquitectura del sistema

```text
                         CLIENTE
                            │
                            ▼
                   ┌────────────────┐
                   │   API GATEWAY  │
                   │      :9000     │
                   └────────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
       ┌────────────┐ ┌────────────┐ ┌────────────┐
       │MS-USUARIOS │ │MS-CATALOGO │ │MS-RECURSOS │
       │   :9001    │ │   :9002    │ │   :9003    │
       └────────────┘ └────────────┘ └────────────┘
              │             │             │
              └─────────────┼─────────────┘
                            │
                            ▼
                     ┌────────────┐
                     │   EUREKA   │
                     │   :8761    │
                     └────────────┘
```

---

# 🧩 Microservicios

| Servicio | Puerto | Función |
|---|---:|---|
| Eureka Server | `8761` | Registro y descubrimiento de servicios |
| API Gateway | `9000` | Punto de entrada hacia los microservicios |
| MS Usuarios | `9001` | Gestión de usuarios y autenticación |
| MS Catálogo | `9002` | Gestión del catálogo de la biblioteca |
| MS Recursos | `9003` | Gestión de recursos |
| Common | - | Componentes compartidos entre microservicios |

---

# ☁️ Despliegue en AWS EC2

El proyecto se encuentra desplegado en una instancia de **Amazon EC2**.

## IP pública de la instancia

```text
54.88.53.218
```

> La IP pública puede cambiar si la instancia EC2 se detiene y vuelve a iniciar, a menos que se utilice una Elastic IP.

---

# 🔐 Reglas de seguridad EC2

Se configuraron las siguientes reglas de entrada en el Security Group de la instancia.

| Tipo | Puerto | Origen |
|---|---:|---|
| Custom TCP | `8761` | Anywhere IPv4 - `0.0.0.0/0` |
| Custom TCP | `9000` | Anywhere IPv4 - `0.0.0.0/0` |
| Custom TCP | `9001` | Anywhere IPv4 - `0.0.0.0/0` |
| Custom TCP | `9002` | Anywhere IPv4 - `0.0.0.0/0` |
| Custom TCP | `9003` | Anywhere IPv4 - `0.0.0.0/0` |

Estas reglas permiten acceder a los servicios desplegados en la instancia EC2 durante el ambiente académico.

---

# 🌐 Acceso a los servicios

## Eureka Server

```text
http://54.88.53.218:8761
```

Desde Eureka es posible comprobar el estado de los distintos servicios registrados.

---

## API Gateway

```text
http://54.88.53.218:9000
```

El API Gateway funciona como punto de entrada principal al sistema.

---

## MS Usuarios

```text
http://54.88.53.218:9001
```

---

## MS Catálogo

```text
http://54.88.53.218:9002
```

---

## MS Recursos

```text
http://54.88.53.218:9003
```

---

# 🗄️ Base de datos

Para el entorno académico del proyecto se utilizan las siguientes credenciales:

```text
Usuario: admin
Contraseña: Duoc.2025
```

Estas credenciales corresponden únicamente al ambiente de laboratorio y evaluación.

> En un entorno productivo las contraseñas no deberían almacenarse directamente en el repositorio. Se recomienda utilizar variables de entorno, GitHub Secrets o AWS Secrets Manager.

---

# 🌿 Control de versiones

El proyecto utiliza **Git** y **GitHub** para administrar y mantener el código fuente.

La rama principal utilizada para esta evaluación es:

```text
main
```

La rama `main` contiene la versión principal y estable del proyecto.

---

# 🌳 Estrategia GitFlow

Para la organización del desarrollo se documenta la estrategia **GitFlow**.

GitFlow permite separar el código estable del código que se encuentra en desarrollo, utilizando distintos tipos de ramas según el propósito de cada cambio.

Su estructura general es:

```text
main
 │
 ├── develop
 │      │
 │      └── feature/<nombre>
 │
 └── hotfix/<nombre>
```

## `main`

Representa la versión estable del proyecto.

```text
main
```

Es la rama utilizada como referencia principal del sistema.

---

## `develop`

Se utiliza como rama de integración para los cambios que todavía se encuentran en desarrollo.

```text
develop
```

Desde esta rama normalmente se crean las nuevas funcionalidades.

---

## `feature/<nombre>`

Las ramas `feature` permiten desarrollar nuevas funcionalidades sin afectar directamente la versión principal.

Ejemplo:

```text
feature/nueva-funcionalidad
```

Su flujo normalmente sería:

```text
develop
   │
   ▼
feature/nueva-funcionalidad
   │
   ▼
develop
```

---

## `hotfix/<nombre>`

Las ramas `hotfix` se utilizan cuando es necesario solucionar rápidamente un problema presente en la versión estable.

Ejemplo:

```text
hotfix/correccion-error
```

Su flujo normalmente sería:

```text
main
  │
  ▼
hotfix/correccion-error
  │
  ▼
main
```

---

# 🔄 Flujo GitFlow

El funcionamiento general de GitFlow se puede representar de la siguiente manera:

```text
                         main
                          ▲
                          │
                       release
                          │
                       develop
                          ▲
                          │
              ┌───────────┴───────────┐
              │                       │
           feature                 feature
              │                       │
              └───────────┬───────────┘
                          │
                       develop
```

En caso de una corrección urgente:

```text
main
 │
 ▼
hotfix
 │
 ▼
main
```

Para esta evaluación el repositorio se mantiene principalmente sobre la rama:

```text
main
```

Mientras que GitFlow se documenta como estrategia recomendada para organizar futuros desarrollos colaborativos.

---

# 📝 Convención de commits

Para mantener un historial ordenado se recomienda utilizar commits claros y descriptivos.

Se utiliza como referencia la convención **Conventional Commits**.

| Tipo | Uso |
|---|---|
| `feat:` | Nueva funcionalidad |
| `fix:` | Corrección de errores |
| `docs:` | Cambios de documentación |
| `ci:` | Cambios relacionados con CI/CD |
| `test:` | Creación o modificación de pruebas |
| `refactor:` | Cambios internos del código |
| `chore:` | Tareas de mantenimiento |

Ejemplos:

```bash
docs: actualiza readme del proyecto
```

```bash
feat: agrega nueva funcionalidad
```

```bash
fix: corrige configuracion del servicio
```

```bash
ci: configura github actions
```

---

# ⚙️ GitHub Actions

GitHub Actions permite automatizar tareas relacionadas con el proyecto.

Los workflows se almacenan dentro de:

```text
.github/workflows/
```

Un workflow puede encargarse de realizar automáticamente tareas como:

```text
Push
  ↓
GitHub Actions
  ↓
Compilación
  ↓
Pruebas
  ↓
Validación
```

Esto permite detectar errores antes de realizar una nueva entrega o despliegue.

---

# 🔁 Integración continua

La integración continua permite validar automáticamente los cambios realizados sobre el proyecto.

El flujo general puede representarse de la siguiente manera:

```text
Desarrollador
      │
      ▼
    Código
      │
      ▼
    Commit
      │
      ▼
     Push
      │
      ▼
    GitHub
      │
      ▼
GitHub Actions
      │
      ▼
Compilación / Pruebas
      │
      ▼
 Resultado
```

Si todas las validaciones finalizan correctamente, el cambio puede considerarse válido.

---

# 🛠️ Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Java | Desarrollo backend |
| Spring Boot | Desarrollo de microservicios |
| Spring Cloud | Comunicación entre servicios |
| Netflix Eureka | Descubrimiento de servicios |
| Spring Cloud Gateway | API Gateway |
| Maven | Gestión de dependencias y compilación |
| MySQL | Base de datos |
| Git | Control de versiones |
| GitHub | Repositorio remoto |
| GitHub Actions | Automatización |
| AWS EC2 | Despliegue en la nube |

---

# 📂 Estructura del proyecto

```text
Biblioteca_DevOps/
│
├── .github/
│   └── workflows/
│
├── api-gateway/
│
├── common/
│
├── eureka/
│
├── init-multi-db/
│
├── ms-catalogo/
│
├── ms-recursos/
│
├── ms-usuarios/
│
├── postman/
│
├── README.md
│
└── pom.xml
```

---

# ▶️ Requisitos

Para trabajar con el proyecto se recomienda tener instalado:

```text
Java
Maven
Git
MySQL
```

Para comprobar las instalaciones:

```bash
java -version
```

```bash
mvn -version
```

```bash
git --version
```

---

# 🔨 Compilación

Desde la carpeta principal del proyecto se puede ejecutar:

```bash
mvn clean install -DskipTests
```

Este comando compila los módulos del proyecto y descarga las dependencias necesarias.

---

# 🚀 Orden de ejecución

Los servicios deben iniciarse en el siguiente orden:

```text
1. Eureka Server       → Puerto 8761

2. MS Usuarios         → Puerto 9001

3. MS Catálogo         → Puerto 9002

4. MS Recursos         → Puerto 9003

5. API Gateway         → Puerto 9000
```

Eureka debe iniciarse primero para permitir que los servicios puedan registrarse correctamente.

---

# ✅ Verificación en AWS

Para comprobar que el proyecto está funcionando en la nube se puede seguir el siguiente procedimiento.

## 1. Verificar EC2

La instancia debe aparecer en AWS con estado:

```text
Running
```

---

## 2. Verificar Eureka

Abrir:

```text
http://54.88.53.218:8761
```

Dentro de Eureka deberían aparecer los servicios registrados.

---

## 3. Verificar los microservicios

Comprobar:

```text
http://54.88.53.218:9001

http://54.88.53.218:9002

http://54.88.53.218:9003
```

---

## 4. Verificar API Gateway

Comprobar:

```text
http://54.88.53.218:9000
```

---

# 🎯 Flujo general DevOps

El flujo aplicado y estudiado dentro del proyecto puede resumirse como:

```text
Desarrollo
    ↓
Git
    ↓
GitHub
    ↓
Control de versiones
    ↓
GitFlow
    ↓
Commits
    ↓
GitHub Actions
    ↓
Integración continua
    ↓
AWS EC2
    ↓
Aplicación desplegada
```

---

# 📚 Ingeniería DevOps

Este proyecto permite aplicar conceptos relacionados con:

- Arquitectura de microservicios.
- Git.
- GitHub.
- Rama `main`.
- GitFlow.
- Convención de commits.
- GitHub Actions.
- Integración continua.
- AWS EC2.
- Reglas de seguridad.
- Despliegue de aplicaciones en la nube.

El objetivo es mantener un proyecto organizado, documentado y preparado para aplicar prácticas modernas de desarrollo y DevOps.# Biblioteca_DevOps
