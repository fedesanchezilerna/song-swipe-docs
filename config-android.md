### **Configuración en Android**

```kotlin
// core/config/SupabaseConfig.kt
object SupabaseConfig {
    const val SUPABASE_URL = "https://your-project.supabase.co"
    const val SUPABASE_KEY = "your-anon-key"
}

// core/di/NetworkModule.kt
@Module
@InstallIn(SingletonIn::class)
object NetworkModule {
    
    @Provides
    @Singleton
    fun provideSupabaseClient(): SupabaseClient {
        return createSupabaseClient(
            supabaseUrl = SupabaseConfig.SUPABASE_URL,
            supabaseKey = SupabaseConfig.SUPABASE_KEY
        ) {
            install(Postgrest)
            install(Storage)
        }
    }
}
```