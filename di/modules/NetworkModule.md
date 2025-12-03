# NetworkModule

Provee clientes HTTP y servicios de API para comunicación con Spotify API.

## Responsabilidades
- Configurar y proveer cliente OkHttp con interceptores
- Proveer instancia de Retrofit configurada para Spotify API
- Proveer interfaz SpotifyApi
- Configurar timeouts, logging y manejo de errores
- Añadir interceptores de autenticación con token de Supabase

## Dependencias provistas

### MVP
- **OkHttpClient**: Cliente HTTP base con interceptores
- **Retrofit**: Cliente REST para Spotify API
- **SpotifyApi**: Interfaz de endpoints de Spotify
- **SpotifyAuthInterceptor**: Inyecta provider_token en headers
- **HttpLoggingInterceptor**: Logging de requests/responses (solo debug)

### Arquitectura
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    
    @Provides
    @Singleton
    fun provideSpotifyAuthInterceptor(
        authRepository: AuthRepository
    ): SpotifyAuthInterceptor = SpotifyAuthInterceptor(authRepository)
    
    @Provides
    @Singleton
    fun provideOkHttpClient(
        spotifyAuthInterceptor: SpotifyAuthInterceptor
    ): OkHttpClient = OkHttpClient.Builder()
        .addInterceptor(spotifyAuthInterceptor)
        .addInterceptor(HttpLoggingInterceptor().apply {
            level = if (BuildConfig.DEBUG) BODY else NONE
        })
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build()
    
    @Provides
    @Singleton
    fun provideRetrofit(okHttpClient: OkHttpClient): Retrofit =
        Retrofit.Builder()
            .baseUrl("https://api.spotify.com/v1/")
            .client(okHttpClient)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    
    @Provides
    @Singleton
    fun provideSpotifyApi(retrofit: Retrofit): SpotifyApi =
        retrofit.create(SpotifyApi::class.java)
}
```

## Consideraciones
- ✅ `@Singleton` para evitar múltiples instancias de clientes HTTP
- ✅ `HttpLoggingInterceptor` solo en debug
- ✅ Timeouts configurados (30s connect/read)
- ✅ SpotifyAuthInterceptor inyecta automáticamente el token
- ✅ Base URL de Spotify centralizada

## Nota sobre Supabase
**Supabase Client** se inicializa en `SupabaseConfig.kt` (no vía Hilt):
```kotlin
val client = SupabaseConfig.client  // Lazy singleton
```