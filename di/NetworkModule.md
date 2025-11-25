# NetworkModule

Provee clientes HTTP, interceptores y servicios de API para comunicación con servicios externos (Spotify, Supabase).

## Responsabilidades
- Configurar y proveer cliente OkHttp con interceptores
- Proveer instancia de Retrofit configurada
- Proveer interfaces de API (SpotifyApi, SupabaseClient)
- Configurar timeouts, logging y manejo de errores
- Añadir interceptores de autenticación y headers

## Dependencias provistas
- **OkHttpClient**: Cliente HTTP base con configuración
- **Retrofit**: Cliente para llamadas REST
- **SpotifyApi**: Interfaz de endpoints de Spotify
- **SupabaseClient**: Cliente de Supabase
- **Interceptores**: SpotifyAuthInterceptor, ErrorInterceptor, HttpLoggingInterceptor

## Consideraciones
- Usar `@Singleton` para clientes (evitar múltiples instancias)
- Configurar timeouts (connect, read, write)
- HttpLoggingInterceptor solo en debug, *no en release*
- Interceptores se añaden en orden: primero auth, luego logging, finalmente error
- Configurar base URLs desde AppConfig