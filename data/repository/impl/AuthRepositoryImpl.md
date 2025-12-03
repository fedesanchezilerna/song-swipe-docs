# AuthRepositoryImpl

Gestiona la autenticación de usuarios usando Supabase con OAuth de Spotify, coordina el flujo de login/logout y mantiene la sesión activa.

## Responsabilidades
- Iniciar sesión con Spotify a través de Supabase OAuth
- Cerrar sesión y limpiar datos locales
- Obtener usuario autenticado actual
- Verificar estado de sesión
- Recuperar provider token de Spotify desde Supabase
- Manejar refresh de tokens (automático via Supabase)

## Dependencias
- **SupabaseClient**: Para operaciones de autenticación OAuth
- **UserDao**: Para guardar/limpiar datos de usuario en caché local
- **UserMapper**: Para convertir SupabaseUser a User de dominio
- **UserPreferencesManager**: Para limpiar preferencias al logout

## Operaciones principales
- `loginWithSpotify()`: Inicia flujo OAuth con Spotify via Supabase
- `logout()`: Cierra sesión y limpia caché local
- `getCurrentUser()`: Retorna usuario autenticado o null
- `isUserLoggedIn()`: Verifica si hay sesión activa
- `getSpotifyToken()`: Obtiene `provider_token` para llamadas a Spotify API

## Flujo de autenticación
1. Usuario hace tap en "Login con Spotify"
2. Supabase redirige a OAuth de Spotify
3. Usuario autoriza en Spotify
4. Supabase recibe callback y guarda tokens
5. Repository obtiene sesión y guarda usuario en Room (caché)
6. Retorna User de dominio al UseCase

## Consideraciones
- Todos los tokens son gestionados por Supabase SDK
- El provider token expira en ~1h, pero Supabase lo refresca automáticamente
- Al hacer logout, limpiar Room pero conservar preferencias de UI
- Manejar errores de red y sesión expirada con NetworkResult