# Estructura de la Arquitectura

Se implementará toda la lógica en el frontend Android, sin backend propio.

**Justificación**:
- Equipo con nivel técnico inicial → Menor curva de aprendizaje
- Spotify API + Supabase proveen toda la funcionalidad necesaria
- Desarrollo más rápido → Un solo lenguaje (Kotlin)
- Sin costos de infraestructura backend
- Clean Architecture se mantiene para buenas prácticas

## Arquitectura Clean Architecture + MVVM

## Estructura de Carpetas

Esta es la estructura objetivo una vez se implementen todas las features:

```
song-swipe-frontend/
  app/src/main/java/org/ilerna/song_swipe_frontend/
    │
    ├── core/                                           # Infraestructura transversal y configuraciones globales
    │   ├── config/                                     # Configuraciones de APIs externas
    │   │   ├── AppConfig.kt                            # Configuración general de la aplicación
    │   │   └── SupabaseConfig.kt                       # Credenciales y setup de Supabase
    │   ├── network/                                    # Manejo de respuestas HTTP y red
    │   │   ├── ApiResponse.kt                          # Wrapper genérico para respuestas de API
    │   │   ├── NetworkResult.kt                        # Sealed class para estados de red (Success, Error, Loading)
    │   │   └── interceptors/                           # Interceptores de Retrofit
    │   │       ├── SpotifyAuthInterceptor.kt           # Inyecta token de Spotify en requests
    │   │       └── ErrorInterceptor.kt                 # Manejo centralizado de errores HTTP
    │   └── utils/                                      # Utilidades y funciones helper
    │       └── Extensions.kt                           # Extensiones de Kotlin para tipos comunes
    │
    ├── data/                                           # Gestión de fuentes de datos y repositorios
    │   ├── datasource/                                 # Fuentes de datos locales y remotas
    │   │   ├── local/                                  # Acceso a datos locales
    │   │   │   ├── dao/                                # Data Access Objects de Room
    │   │   │   ├── database/                           # Configuración de Room Database
    │   │   │   │   └── DatabaseConstants.kt            # Nombres de tablas y columnas
    │   │   │   └── preferences/                        # SharedPreferences y DataStore
    │   │   └── remote/                                 # Acceso a APIs remotas
    │   │       ├── api/                                # Interfaces de Retrofit
    │   │       │   ├── SpotifyApi.kt                   # Endpoints de Spotify Web API
    │   │       │   └── SupabaseClient.kt               # Cliente de Supabase
    │   │       └── dto/                                # Data Transfer Objects (respuestas de API)
    │   │           ├── spotify/                        # DTOs de Spotify (tracks, albums, artists)
    │   │           ├── song/                           # DTOs de canciones
    │   │           └── playlist/                       # DTOs de playlists
    │   ├── local/                                      # Entidades de base de datos local
    │   │   ├── entity/                                 # Entidades de Room (tablas)
    │   │   └── converters/                             # Type converters de Room
    │   ├── remote/                                     # Implementaciones de datasources remotos
    │   │   └── impl/
    │   │       ├── SpotifyDataSourceImpl.kt            # Implementación de llamadas a Spotify API
    │   │       └── SupabaseDataSourceImpl.kt           # Implementación de operaciones con Supabase
    │   └── repository/                                 # Implementaciones de repositorios
    │       ├── impl/                                   # Coordinan fuentes de datos local y remota
    │       │   ├── AuthRepositoryImpl.kt               # Lógica de autenticación con Supabase
    │       │   ├── SongRepositoryImpl.kt               # CRUD de canciones (Room + Spotify)
    │       │   ├── PlaylistRepositoryImpl.kt           # CRUD de playlists (Room + Supabase)
    │       │   ├── UserRepositoryImpl.kt               # Gestión de perfil de usuario
    │       │   └── SwipeRepositoryImpl.kt              # Gestión de swipes (Room + Supabase)
    │       └── mapper/                                 # Conversión entre DTOs y entidades de dominio
    │           ├── SupabaseUserMapper.kt               # SupabaseUser → User
    │           ├── SongMapper.kt                       # SongDTO → Song
    │           ├── PlaylistMapper.kt                   # PlaylistDTO → Playlist
    │           └── UserMapper.kt                       # UserDTO → User
    │
    ├── domain/                                         # Lógica de negocio pura (independiente de frameworks)
    │   ├── model/                                      # Entidades, Value Objects, estados, enums
    │   │   └── DomainConstants.kt                      # Constantes de lógica de negocio y validación
    │   ├── repository/                                 # Interfaces de repositorio (contratos)
    │   │   └── AuthRepository.kt                       # Contrato de autenticación
    │   └── usecase/                                    # Casos de uso con lógica de negocio
    │       ├── auth/                                   # Casos de uso de autenticación
    │       │   ├── LoginUseCase.kt                     # Validar y autenticar usuario
    │       │   ├── LogoutUseCase.kt                    # Cerrar sesión y limpiar datos
    │       │   └── GetCurrentUserUseCase.kt            # Obtener usuario autenticado
    │       ├── song/                                   # Casos de uso de canciones
    │       ├── swipe/                                  # Casos de uso de swipes (like/dislike)
    │       ├── playlist/                               # Casos de uso de playlists
    │       └── user/                                   # Casos de uso de perfil de usuario
    │
    ├── presentation/                                   # Interfaz de usuario y estado de UI
    │   ├── components/                                 # Componentes reutilizables de Jetpack Compose
    │   │   ├── LoadingIndicator.kt                     # Indicador de carga
    │   │   ├── PrimaryButton.kt                        # Botón primario
    │   │   └── SecundaryButton.kt                      # Botón secundario
    │   ├── screen/                                     # Pantallas completas de la app (organizadas por feature)
    │   │   ├── login/                                  # Feature: Autenticación
    │   │   │   ├── LoginScreen.kt                      # Pantalla de login
    │   │   │   ├── LoginErrorScreen.kt                 # Pantalla de error
    │   │   │   └── LoginViewModel.kt                   # ViewModel del login
    │   │   ├── swipe/                                  # Feature: Swipe de canciones
    │   │   │   ├── SwipeScreen.kt                      # Pantalla de swipe
    │   │   │   ├── SwipeViewModel.kt                   # ViewModel del swipe
    │   │   │   └── SwipeState.kt                       # Estado del swipe
    │   │   ├── playlist/                               # Feature: Gestión de playlists
    │   │   │   ├── PlaylistScreen.kt                   # Pantalla de playlists
    │   │   │   └── PlaylistViewModel.kt                # ViewModel de playlists
    │   │   ├── profile/                                # Feature: Perfil de usuario
    │   │   │   ├── ProfileScreen.kt                    # Pantalla de perfil
    │   │   │   └── ProfileViewModel.kt                 # ViewModel del perfil
    │   │   └── main/                                   # Feature: Navegación principal
    │   │       └── MainScreen.kt                       # Scaffold principal con bottom nav
    │   ├── theme/                                      # Temas de Material Design
    │   │   ├── Color.kt                                # Paleta de colores de la app
    │   │   ├── Theme.kt                                # Configuración de tema Material 3
    │   │   ├── Type.kt                                 # Tipografía de la app
    │   │   └── Dimensions.kt                           # Dimensiones y espaciados
    │   └── utils/                                      # Utilidades específicas de UI
    │       └── UIConstants.kt                          # Constantes de UI (animaciones, límites, dimensiones)
    │
    ├── di/                                             # Inyección de dependencias con Hilt
    │   ├── AppModule.kt                                # Provee dependencias generales de la app
    │   ├── DatabaseModule.kt                           # Provee Room Database y DAOs
    │   ├── NetworkModule.kt                            # Provee Retrofit, OkHttp, interceptores
    │   ├── RepositoryModule.kt                         # Provee implementaciones de repositorios
    │   └── UseCaseModule.kt                            # Provee casos de uso
    │
    └── MainActivity.kt                                 # Activity principal (punto de entrada)
```

---

## 📊 Análisis de Implementación Actual

### ✅ **Aspectos Positivos**

1. **Nomenclatura correcta**: Se usa `presentation/` en lugar de `ui/` (estándar Android)
2. **Organización por feature**: `LoginViewModel.kt` está junto a `LoginScreen.kt` (cohesión)
3. **Separación de capas**: Core, Data, Domain, Presentation están bien diferenciadas
4. **Uso de StateFlow**: ViewModel implementa flujo reactivo con StateFlow
5. **Sealed classes**: `AuthState` maneja estados de forma type-safe
6. **Clean Architecture**: Dependencias apuntan hacia adentro (Data → Domain ← Presentation)

### **Mejoras Requeridas (Próximas Tareas)**

#### 1. **Implementar Dependency Injection (CRÍTICO)**
```
📦 Agregar a build.gradle.kts:
- Hilt Android
- Hilt Navigation Compose

📁 Crear módulo di/:
- AppModule.kt
- RepositoryModule.kt
- UseCaseModule.kt
```

**Sin DI:**
- ❌ Acoplamiento fuerte entre capas
- ❌ Testing difícil (no puedes mockear dependencias)
- ❌ Instanciación manual de objetos
- ❌ No escalable

#### 2. **Agregar Navigation Compose**
```kotlin
// Necesitas implementar:
presentation/navigation/
├── Screen.kt           // Sealed class con rutas
├── AppNavigation.kt    // NavHost configuration
└── NavigationExtensions.kt
```

#### 3. **Implementar manejo de eventos (UDF completo)**
```kotlin
// Patrón recomendado por cada screen:
presentation/screen/login/
├── LoginScreen.kt
├── LoginViewModel.kt
├── LoginState.kt       // data class con estado de UI
└── LoginEvent.kt       // sealed interface con eventos
```


### 📝 **Notas de Arquitectura**

#### **Flujo de Dependencias**
```
┌─────────────────┐
│  Presentation   │  (ViewModels, Screens)
└────────┬────────┘
         │ depende de ↓
┌────────▼────────┐
│     Domain      │  (UseCases, Models, Repository Interfaces)
└────────┬────────┘
         │ implementado por ↓
┌────────▼────────┐
│      Data       │  (Repository Implementations, DataSources)
└─────────────────┘
```

#### **Regla de Oro**
> **Domain NO debe depender de Data ni Presentation**
> 
> Las interfaces de repositorio siempre van en `domain/` porque definen **QUÉ** hace el sistema, no **CÓMO** lo hace.