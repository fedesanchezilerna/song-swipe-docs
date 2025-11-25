# Capas de la Arquitectura

## Descripción de Capas

### **1. Core Layer**
Contiene configuraciones, utilidades y componentes transversales que se usan en toda la aplicación.
- **config/**: Configuraciones de la app (Spotify, Supabase, etc.)
- **network/**: Manejo de respuestas HTTP, interceptores de autenticación
- **utils/**: Funciones de utilidad, extensiones, constantes

### **2. Data Layer**
Maneja todas las fuentes de datos (local y remota) y provee datos al dominio.
- **datasource/**: Fuentes de datos (Room, Retrofit APIs)
- **local/**: Base de datos local (Room), DAOs, entidades
- **remote/**: APIs remotas, DTOs
- **repository/**: Implementaciones de repositorios que coordinan local y remoto
- **mapper/**: Conversión de DTOs a entidades de dominio

### **3. Domain Layer**
Núcleo de la lógica de negocio, independiente de frameworks y librerías externas.
- **model/**: Entidades de negocio, Value Objects, estados y enums
- **repository/**: Interfaces de repositorios (contratos)
- **usecase/**: Casos de uso que encapsulan lógica de negocio

### **4. Presentation Layer**
Maneja la UI y la interacción con el usuario.
- **components/**: Componentes reutilizables de Jetpack Compose
- **screens/**: Pantallas completas de la aplicación
- **viewmodels/**: ViewModels que manejan el estado de la UI

### **5. UI Layer**
Configuración de temas y recursos visuales.
- **theme/**: Colores, tipografía, formas del Material Design

---

## 📋 Responsabilidades por Capa

### **Core Layer (core/)**
**Propósito**: Proveer infraestructura transversal y configuraciones globales.

**Responsabilidades**:
- ✅ Configurar credenciales de APIs externas (Spotify, Supabase)
- ✅ Definir constantes y valores compartidos en toda la app
- ✅ Implementar interceptores HTTP para autenticación y errores
- ✅ Proveer tipos genéricos para manejo de respuestas de red
- ✅ Ofrecer funciones de extensión y utilidades reutilizables
- ❌ **No debe** contener lógica de negocio
- ❌ **No debe** depender de las capas de dominio o presentación

**Componentes principales**:
- `SupabaseConfig`: Credenciales y configuración de Supabase
- `SpotifyAuthInterceptor`: Inyecta token de Supabase en llamadas a Spotify API
- `NetworkResult<T>`: Wrapper para respuestas HTTP (Success, Error, Loading)
- `Extensions.kt`: Extensiones de Kotlin para tipos comunes

---

### **Data Layer (data/)**
**Propósito**: Gestionar todas las fuentes de datos y proveer implementaciones de repositorios.

**Responsabilidades**:
- ✅ Implementar interfaces de repositorio definidas en Domain
- ✅ Coordinar entre fuentes de datos locales (Room) y remotas (APIs)
- ✅ Mapear DTOs de red a entidades de dominio
- ✅ Gestionar caché local con Room para modo offline
- ✅ Sincronizar datos entre Room y Supabase
- ✅ Manejar errores de red y transformarlos en resultados comprensibles
- ❌ **No debe** contener lógica de negocio compleja
- ❌ **No debe** depender de la capa de presentación

**Componentes principales**:
- `AuthRepositoryImpl`: Implementa autenticación con SupabaseClient
- `SpotifyDataSourceImpl`: Llamadas a Spotify Web API (canciones, playlists)
- `SupabaseDataSourceImpl`: Sincronización de datos con Supabase PostgreSQL
- `SupabaseUserMapper`: Convierte `SupabaseUser` → `User` (entidad de dominio)
- `SongDao`, `UserDao`, `SwipeDao`: Acceso a base de datos Room

---

### **Domain Layer (domain/)**
**Propósito**: Contener la lógica de negocio pura, independiente de frameworks.

**Responsabilidades**:
- ✅ Definir entidades de negocio (User, Song, Playlist, Swipe)
- ✅ Definir interfaces de repositorio (contratos sin implementación)
- ✅ Implementar casos de uso con lógica de negocio específica
- ✅ Validar reglas de negocio (ej: usuario debe tener email válido)
- ✅ Orquestar operaciones entre múltiples repositorios
- ❌ **No debe** depender de Android (Context, Activity, etc.)
- ❌ **No debe** depender de librerías externas (Retrofit, Room, etc.)
- ❌ **No debe** conocer detalles de implementación (APIs, BD)

**Componentes principales**:
- `User`, `Song`, `Playlist`: Entidades de negocio puras (POJOs)
- `AuthRepository`: Interface que define operaciones de autenticación
- `LoginUseCase`: Lógica de negocio para login (validar, autenticar, guardar)
- `SwipeSongUseCase`: Lógica para procesar swipe (guardar, validar duplicados)

---

### **Presentation Layer (presentation/)**
**Propósito**: Manejar la interfaz de usuario y el estado de la UI.

**Responsabilidades**:
- ✅ Renderizar componentes visuales con Jetpack Compose
- ✅ Manejar eventos de usuario (clicks, swipes, inputs)
- ✅ Gestionar estado de UI en ViewModels
- ✅ Invocar casos de uso del dominio
- ✅ Observar cambios de estado y actualizar la UI reactivamente
- ✅ Mostrar mensajes de error y estados de carga
- ❌ **No debe** contener lógica de negocio
- ❌ **No debe** acceder directamente a repositorios (usar use cases)
- ❌ **No debe** hacer llamadas directas a APIs

**Componentes principales**:
- `LoginScreen`: Pantalla de login con formulario y estados
- `LoginViewModel`: Gestiona estado de autenticación y llama a `LoginUseCase`
- `SwipeCard`: Componente reutilizable para mostrar canciones
- `PlaylistViewModel`: Gestiona lista de playlists y operaciones CRUD

---

### **UI Layer (ui/)**
**Propósito**: Definir tema visual y estilos de Material Design.

**Responsabilidades**:
- ✅ Configurar paleta de colores (Light/Dark mode)
- ✅ Definir tipografía y tamaños de texto
- ✅ Configurar formas y elevaciones de componentes
- ❌ **No debe** contener lógica de negocio ni de presentación

**Componentes principales**:
- `Color.kt`: Paleta de colores primarios, secundarios, surface
- `Theme.kt`: Configuración de MaterialTheme
- `Type.kt`: Estilos de tipografía (heading, body, caption)

---

### **DI Layer (di/)**
**Propósito**: Configurar inyección de dependencias con Hilt.

**Responsabilidades**:
- ✅ Proveer instancias singleton de APIs y clientes (Retrofit, Supabase)
- ✅ Proveer instancias de Room Database y DAOs
- ✅ Proveer implementaciones de repositorios
- ✅ Proveer casos de uso con sus dependencias inyectadas
- ✅ Configurar interceptores de red
- ❌ **No debe** contener lógica de negocio

**Componentes principales**:
- `NetworkModule`: Provee SupabaseClient, SpotifyApi, interceptores
- `DatabaseModule`: Provee Room Database y DAOs
- `RepositoryModule`: Provee implementaciones de repositorios
- `UseCaseModule`: Provee casos de uso