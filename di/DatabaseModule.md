# DatabaseModule

Provee instancias de Room Database y todos los DAOs necesarios para acceso a datos locales.

## Responsabilidades
- Proveer instancia singleton de `AppDatabase`
- Construir base de datos con Room.databaseBuilder
- Proveer cada DAO (SongDao, PlaylistDao, SwipeDao, UserDao)
- Configurar callbacks de base de datos si es necesario

## Dependencias provistas
- **AppDatabase**: Instancia principal de Room
- **SongDao**: Acceso a tabla de canciones
- **PlaylistDao**: Acceso a tabla de playlists
- **SwipeDao**: Acceso a tabla de swipes
- **UserDao**: Acceso a tabla de usuario

## Consideraciones
- Usar `@Singleton` para AppDatabase (única instancia)
- Los DAOs se obtienen desde AppDatabase, no se crean directamente
- Configurar `.fallbackToDestructiveMigration()` solo en desarrollo