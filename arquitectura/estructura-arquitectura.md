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
    │       ├── Constants.kt                            # Constantes globales (URLs, keys, etc.)
    │       └── Extensions.kt                           # Extensiones de Kotlin para tipos comunes\
    │
    ├── data/                                           # Gestión de fuentes de datos y repositorios
    │   ├── datasource/                                 # Fuentes de datos locales y remotas
    │   │   ├── local/                                  # Acceso a datos locales
    │   │   │   ├── dao/                                # Data Access Objects de Room
    │   │   │   ├── database/                           # Configuración de Room Database
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
    │   ├── repository/                                 # Interfaces de repositorio (contratos)
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
    │   ├── screens/                                    # Pantallas completas de la app
    │   │   ├── auth/                                   # Pantallas de autenticación (login, registro)
    │   │   ├── swipe/                                  # Pantalla de swipe de canciones
    │   │   ├── playlist/                               # Pantallas de gestión de playlists
    │   │   ├── profile/                                # Pantalla de perfil de usuario
    │   │   └── main/                                   # Pantalla principal y navegación
    │   │       └── MainScreen.kt                       # Scaffold principal con bottom nav
    │   └── viewmodels/                                 # ViewModels que gestionan estado de UI
    │
    ├── ui/                                             # Configuración de tema visual
    │   └── theme/                                      # Temas de Material Design
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