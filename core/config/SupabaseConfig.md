# SupabaseConfig

Centraliza la configuración de Supabase, inicializa el cliente y gestiona autenticación OAuth con Spotify.

## Responsabilidades
- Almacenar URL del proyecto y anon key de Supabase
- Inicializar cliente de Supabase Auth
- Configurar deep link para OAuth callback (`songswipe://callback`)
- Configurar auto-refresh de tokens
- Proveer instancia singleton del cliente

## Implementación

```kotlin
object SupabaseConfig {
    
    const val SUPABASE_URL = "https://keogusadivocspsdysez.supabase.co"
    const val SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    
    val client: SupabaseClient by lazy {
        createSupabaseClient(
            supabaseUrl = SUPABASE_URL,
            supabaseKey = SUPABASE_ANON_KEY
        ) {
            install(Auth) {
                scheme = "songswipe"
                host = "callback"
            }
            
            // MVP: No se usa Postgrest (no hay tabla users custom)
            // install(Postgrest)
        }
    }
}
```

## Plugins Instalados

### Auth (Implementado)
```kotlin
install(Auth) {
    scheme = "songswipe"     // Deep link scheme
    host = "callback"         // Deep link host
}
```

Permite:
- OAuth con Spotify
- Gestión de sesiones
- Auto-refresh de tokens
- Provider token para Spotify API

### Postgrest (No en MVP)
```kotlin
// Post-MVP: Para interactuar con tablas custom
// install(Postgrest)
```

Se considera agregar cuando se cree la tabla `public.users` personalizada.

## Consideraciones

### Seguridad
- ✅ `SUPABASE_ANON_KEY` es segura para cliente
- ⚠️ En prod, cargar desde `local.properties` o variables de entorno
- ❌ **NUNCA** commitear claves en Git

### Deep Link
- Debe coincidir con `AndroidManifest.xml`:
  ```xml
  <data android:scheme="songswipe" android:host="callback" />
  ```
- Debe estar en whitelist de Supabase Dashboard

### Session Management
Supabase maneja automáticamente:
- Persistencia de sesión (sobrevive reinicios)
- Refresh de tokens expirados
- Storage seguro en SharedPreferences

## Uso en la App

```kotlin
// Obtener sesión actual
val session = SupabaseConfig.client.auth.currentSessionOrNull()

// Obtener usuario
val user = SupabaseConfig.client.auth.currentUserOrNull()

// Obtener provider token (para Spotify API)
val spotifyToken = session?.providerToken

// Sign out
SupabaseConfig.client.auth.signOut()
```