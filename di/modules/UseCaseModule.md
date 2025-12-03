# UseCaseModule

Provee instancias de casos de uso con sus dependencias inyectadas, encapsulando lógica de negocio específica.

## Responsabilidades
- Proveer casos de uso de autenticación (LoginUseCase, LogoutUseCase, etc.)
- Proveer casos de uso de canciones y swipes
- Proveer casos de uso de playlists
- Proveer casos de uso de gestión de usuario
- Inyectar repositorios necesarios en cada caso de uso

## Casos de uso esperados
**Autenticación:**
- LoginUseCase, LogoutUseCase, GetCurrentUserUseCase

**Canciones y Swipes:**
- GetRandomSongsUseCase, SwipeSongUseCase, GetSwipeHistoryUseCase

**Playlists:**
- CreatePlaylistUseCase, GetUserPlaylistsUseCase, AddSongToPlaylistUseCase

**Usuario:**
- GetUserProfileUseCase, UpdateUserProfileUseCase

## Consideraciones
- Casos de uso no son singleton, se crean nuevas instancias cuando se necesitan
- Cada UseCase debe tener responsabilidad única y clara
- Inyectar solo los repositorios que realmente necesita
- Los ViewModels consumen UseCases, no repositorios directamente