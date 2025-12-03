# RepositoryModule

Provee implementaciones concretas de repositorios, vinculándolas con sus interfaces del dominio.

## Responsabilidades
- Proveer implementaciones de repositorios (AuthRepositoryImpl, SongRepositoryImpl, etc.)
- Vincular interfaces de dominio con implementaciones de data
- Inyectar dependencias necesarias (DAOs, APIs, mappers)
- **Garantizar que el dominio solo conozca interfaces, no implementaciones**

## Dependencias provistas
- **AuthRepository**: Autenticación y gestión de sesión
- **SongRepository**: CRUD de canciones (local + remoto)
- **PlaylistRepository**: CRUD de playlists
- **SwipeRepository**: Gestión de swipes
- **UserRepository**: Gestión de perfil de usuario

## Patrón de binding
Usar `@Binds` para vincular interfaz con implementación (más eficiente que `@Provides`):
- La función es abstracta
- Recibe la implementación como parámetro
- Retorna la interfaz

## Consideraciones
- Los repositorios coordinan datasources locales y remotos
- Usar `@Singleton` si el repositorio mantiene estado
- *No incluir lógica de negocio*, solo coordinación de fuentes de datos

## Ejemplo conceptual
```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds
    @Singleton
    abstract fun bindAuthRepository(
        impl: AuthRepositoryImpl
    ): AuthRepository

    @Binds
    @Singleton
    abstract fun bindSongRepository(
        impl: SongRepositoryImpl
    ): SongRepository

    // Más bindings de repositorios...
}
```