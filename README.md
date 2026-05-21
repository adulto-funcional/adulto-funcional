<div align="center">

```
 █████╗ ██████╗ ██╗   ██╗██╗  ████████╗ ██████╗ 
██╔══██╗██╔══██╗██║   ██║██║  ╚══██╔══╝██╔═══██╗
███████║██║  ██║██║   ██║██║     ██║   ██║   ██║
██╔══██║██║  ██║██║   ██║██║     ██║   ██║   ██║
██║  ██║██████╔╝╚██████╔╝███████╗██║   ╚██████╔╝
╚═╝  ╚═╝╚═════╝  ╚═════╝ ╚══════╝╚═╝    ╚═════╝ 

███████╗██╗   ██╗███╗   ██╗ ██████╗██╗ ██████╗ ███╗   ██╗ █████╗ ██╗     
██╔════╝██║   ██║████╗  ██║██╔════╝██║██╔═══██╗████╗  ██║██╔══██╗██║     
█████╗  ██║   ██║██╔██╗ ██║██║     ██║██║   ██║██╔██╗ ██║███████║██║     
██╔══╝  ██║   ██║██║╚██╗██║██║     ██║██║   ██║██║╚██╗██║██╔══██║██║     
██║     ╚██████╔╝██║ ╚████║╚██████╗██║╚██████╔╝██║ ╚████║██║  ██║███████╗
╚═╝      ╚═════╝ ╚═╝  ╚═══╝ ╚═════╝╚═╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝  ╚═╝╚══════╝
```

### _Finanzas, agenda y seguridad digital. Todo en un solo lugar._

[![Java](https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![React Native](https://img.shields.io/badge/React_Native-0.81-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactnative.dev)
[![Expo](https://img.shields.io/badge/Expo-SDK_54-000020?style=for-the-badge&logo=expo&logoColor=white)](https://expo.dev)
[![MariaDB](https://img.shields.io/badge/MariaDB-11.8-003545?style=for-the-badge&logo=mariadb&logoColor=white)](https://mariadb.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)

</div>

---

## El problema

Convertirse en adulto no viene con manual. De repente tenés que recordar el vencimiento del arriendo, saber si podés salir a comer este fin de semana, no olvidar la contraseña del banco y cumplir los compromisos que te pusiste.

**Adulto Funcional** es la herramienta que centraliza todo eso. Finanzas personales, agenda con rachas de cumplimiento y un gestor de contraseñas cifrado — accesible desde el celular, la web o el escritorio, sincronizado en un servidor privado.

---

## Qué hace la app

```
┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
│   💰 FINANZAS   │   │  📅  AGENDA     │   │  🔐 PASSWORDS   │   │  📊 DASHBOARD   │
│─────────────────│   │─────────────────│   │─────────────────│   │─────────────────│
│ Ingresos        │   │ Compromisos     │   │ Gestor cifrado  │   │ Saldo actual    │
│ Egresos         │   │ con prioridad   │   │ con Master Key  │   │ Próx. gastos    │
│ Gastos fijos    │   │ Recurrencia     │   │ AES-256         │   │ Compromisos     │
│ Categorías      │   │ Racha individual│   │ Salt + IV único │   │ Racha global    │
│ Historial       │   │ Recordatorios   │   │ por credencial  │   │ Gráficos 3 m.   │
└─────────────────┘   └─────────────────┘   └─────────────────┘   └─────────────────┘
```

### Finanzas personales
Registrá ingresos y egresos con categorías personalizables, monto en formato COP, fecha y descripción. Los **gastos fijos** (Netflix, arriendo, gym) se gestionan por separado: tienen frecuencia (semanal, mensual, anual), próxima fecha de vencimiento y días de anticipación para el recordatorio. Pagar un gasto fijo desde la app registra automáticamente el egreso y recalcula la próxima fecha.

### Agenda y compromisos
Cada evento tiene título, categoría, prioridad (alta / media / baja), hora de inicio y fin, frecuencia y recordatorio. Lo más importante: cada evento recurrente lleva su propia **racha** — un contador de días consecutivos que aumenta al marcarlo como completado y se reinicia si lo dejás pasar. La racha global mide días consecutivos de inicio de sesión.

### Gestor de contraseñas
Las contraseñas de servicios externos se cifran con **AES-256** en el backend usando una derivación de la Master Key del usuario. La Master Key nunca se guarda en texto plano — solo su hash Argon2. Sin Master Key verificada, el gestor no abre. Un atacante con acceso completo a la base de datos no puede descifrar nada.

---

## Arquitectura

El sistema sigue un modelo **cliente–servidor** estricto: el frontend solo muestra e interactúa, el backend concentra toda la lógica de negocio, seguridad y persistencia.

```
┌──────────────────────────────────────────────────────────┐
│                      CLIENTES                            │
│   📱 React Native (Expo)   🌐 Web (Expo Router)         │
│              🖥️  Desktop (futuro)                        │
└───────────────────────┬──────────────────────────────────┘
                        │  HTTPS · JWT en HttpOnly Cookie
                        ▼
┌──────────────────────────────────────────────────────────┐
│               BACKEND — Spring Boot 3.5                  │
│                                                          │
│  auth/   account/   finances/   agenda/   security/      │
│                                                          │
│  Spring Security · JWT · Argon2 · AES-256 · Jsoup XSS   │
└───────────────────────┬──────────────────────────────────┘
                        │  JPA / Hibernate · Flyway
                        ▼
┌──────────────────────────────────────────────────────────┐
│          MariaDB 11.8          │         Redis            │
│   accounts, movements,         │   Sesiones Master Key    │
│   fixed_expenses, events,      │   (producción)           │
│   categories, passwords        │                          │
└──────────────────────────────────────────────────────────┘
```

### Backend: Clean Architecture

```
org.adultofuncional.main/
├── account/        # Perfil, edición de datos, eliminación de cuenta
├── auth/           # Login, register, logout, recuperación de contraseña
├── config/         # Spring Security, CORS, JWT, ClientTypeResolver
├── finances/       # Movimientos, gastos fijos, categorías
├── agenda/         # Eventos con frecuencia, prioridad y rachas
├── security/       # Gestor de contraseñas cifrado con Master Key
└── shared/         # ApiResponse, excepciones, validación XSS (@NoHtml)
```

Cada módulo sigue tres capas internas:

```
domain/         → modelos y puertos (interfaces de repositorio)
application/    → casos de uso y DTOs
infrastructure/ → controladores REST, entidades JPA, mappers
```

### Frontend: Expo Router (basado en archivos)

```
app/
├── (auth)/             # Login, Register, ForgotPassword, ResetPassword
└── (app)/              # Pantallas autenticadas
    ├── finances/        # CRUD movimientos + gastos fijos
    ├── compromises/     # CRUD eventos con rachas
    ├── passwords/       # Gestor de contraseñas + Master Key
    └── profile/         # Estadísticas, configuración, logout
src/
├── api/                # Clientes Axios por módulo
├── hooks/              # Lógica de negocio reutilizable
├── components/         # BottomNav, modales, UI compartida
└── services/           # storage, dateUtils, currencyUtils, streakService
```

---

## Stack completo

### Backend

| Tecnología | Uso |
|---|---|
| Java 21 | Lenguaje principal |
| Spring Boot 3.5 | Framework backend |
| Spring Security | Autenticación y autorización |
| Spring Data JPA + Hibernate | ORM y acceso a datos |
| MariaDB 11.8 | Base de datos principal |
| Flyway | Migraciones de esquema versionadas |
| JWT | Autenticación stateless |
| Argon2 | Hash de contraseñas de login y Master Key |
| AES-256-GCM | Cifrado de contraseñas del gestor |
| Redis | Sesiones de Master Key en producción |
| Jsoup | Validación anti-XSS en todos los campos de texto |
| Testcontainers | Pruebas de integración con MariaDB real |
| Lombok | Reducción de boilerplate |
| Maven | Build y dependencias |

### Frontend

| Tecnología | Uso |
|---|---|
| React Native 0.81 | UI nativa multiplataforma |
| Expo SDK 54 | Toolchain, builds y APIs nativas |
| TypeScript | Tipado estático |
| Expo Router | Navegación basada en sistema de archivos |
| Axios | Cliente HTTP con interceptores JWT |
| Expo SecureStore | Almacenamiento seguro del token JWT |
| React Native Chart Kit | Gráficos de ingresos/egresos |
| AsyncStorage | Caché local de datos de sesión |

---

## Base de datos

El esquema completo se gestiona con Flyway. Las relaciones principales:

```
accounts (entidad central)
    ├── movements        (ingresos y egresos)
    ├── fixed_expenses   (gastos fijos recurrentes)
    ├── events           (compromisos de agenda)
    └── passwords        (credenciales cifradas)

categories               (clasifican movimientos y eventos)
```

Todos los IDs son **UUID v7** — ordenables temporalmente, generados desde la aplicación. El borrado en cascada se maneja desde JPA, no desde SQL, para mantener el control en el dominio.

Las contraseñas del gestor usan un `salt` y un `IV` únicos por registro. Sin la Master Key del usuario, el ciphertext es indescifrable incluso con acceso directo a la base de datos.

---

## Seguridad

```
Autenticación   ─── JWT en HttpOnly Cookie + body para clientes nativos
Contraseñas     ─── Argon2 (login) · AES-256-GCM (gestor)
Entrada         ─── @NoHtml con Jsoup en todos los DTOs (anti-XSS stored)
Ownership       ─── OwnershipValidator: cada recurso valida que pertenece al usuario JWT
Errores         ─── No enumera usuarios: login incorrecto = 401 genérico siempre
Master Key      ─── Solo hash Argon2 en BD · clave real nunca persiste
```

---

## Instalación

### Requisitos

- Java 21+
- Maven 3.9+ (o usar el wrapper incluido)
- MariaDB 11.8
- Redis (para producción; en dev se usa in-memory)
- Node.js 18+ y Expo CLI

### Backend

```bash
git clone <backend-repo>
cd adulto-funcional-server

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales

# Ejecutar
./mvnw spring-boot:run
```

Variables de entorno clave:

| Variable | Descripción | Ejemplo |
|---|---|---|
| `MARIADB_DATABASE` | Nombre de la BD | `adulto_funcional` |
| `MARIADB_USER` | Usuario de la app | `afs_user` |
| `MARIADB_PASSWORD` | Contraseña del usuario | `userpass` |
| `JWT_SECRET` | Clave de firma JWT (mín. 32 chars) | `mi-clave-super-secreta` |
| `JWT_EXPIRATION` | Expiración en ms | `86400000` |
| `CORS_ALLOWED_ORIGINS` | Orígenes permitidos | `http://localhost:8081` |
| `APP_COOKIE_SECURE` | Cookie Secure en HTTPS | `true` |

### Frontend

```bash
git clone <frontend-repo>
cd adulto-funcional-movil

npm install

# Configurar la URL del backend
# src/constants/config.ts
# export const API_BASE_URL = 'http://192.168.1.100:8080'

npx expo start
```

Escaneá el QR con Expo Go para probar en tu dispositivo, o abrí en el navegador para la versión web.

### Docker (producción)

```bash
docker compose up -d
```

El `docker-compose.yml` levanta MariaDB, Redis y el backend con healthchecks encadenados. La app espera que la base de datos esté lista antes de arrancar. Flyway aplica las migraciones automáticamente al iniciar.

### Build APK

```bash
# Con EAS Build (recomendado)
eas build -p android --profile preview

# O localmente con Android SDK
npx expo prebuild --clean
cd android && ./gradlew assembleRelease
```

---

## Estado del proyecto

| Módulo | Estado |
|---|---|
| Autenticación (login, register, logout, recuperación) | ✅ Completo |
| Gestión de cuenta (perfil, edición, eliminación) | ✅ Completo |
| Finanzas (movimientos, gastos fijos, categorías) | ✅ Completo |
| Agenda (eventos, frecuencia, rachas) | ✅ Completo |
| Gestor de contraseñas (AES-256 + Master Key) | ✅ Completo |
| Dashboard (saldo, gráficos, racha global) | ✅ Completo |
| Pruebas de integración con Testcontainers | ✅ Completo |
| Modo oscuro | ⚠️ Implementado, desactivado por error de compilación |
| Notificaciones push | 🔲 Pendiente (requiere development build) |
| Recuperación de contraseña real (email) | 🔲 Pendiente (EmailJS o backend) |
| Build para iOS | 🔲 Pendiente (requiere cuenta Apple Developer) |

---

## Equipo

```

┌──────────────────────────────┐
│       ADULTO FUNCIONAL       │
├──────────────────────────────┤
│  Miguel Angel Blandón Montes │
│  Juan Sebastián Ríos Rodrígu │
│  Lidys Esther Jaraba         │
│  Daniel Zapata               │
└──────────────────────────────┘

```

---

<div align="center">

```
INGRESOS ──────────────────────────────────────────► CONTROL
EGRESOS  ──────────────────────────────────────────► CONTROL
GASTOS   ──────────────────────────────────────────► CONTROL
AGENDA   ──────────────────────────────────────────► CONTROL
CLAVES   ──────────────────────────────────────────► CONTROL
```

_Adulto funcional. Modo: activado._ 🟣

</div>
