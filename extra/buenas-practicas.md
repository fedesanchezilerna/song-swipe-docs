## 💡 Mejores Prácticas para Equipo Junior

### **1. Separación de Responsabilidades**
```kotlin
// ❌ MAL: Todo en el ViewModel
class SwipeViewModel {
    fun swipeSong() {
        // Lógica de negocio
        // Llamada a API
        // Guardado en BD
        // Actualización de UI
    }
}

// ✅ BIEN: Cada capa con su responsabilidad
class SwipeViewModel(
    private val swipeSongUseCase: SwipeSongUseCase
) {
    fun swipeSong(song: Song, direction: SwipeDirection) {
        viewModelScope.launch {
            swipeSongUseCase(song, direction)
                .onSuccess { /* actualizar UI */ }
                .onFailure { /* mostrar error */ }
        }
    }
}
```

### **2. Manejo de Errores Consistente**
```kotlin
// core/network/NetworkResult.kt
sealed class NetworkResult<out T> {
    data class Success<T>(val data: T) : NetworkResult<T>()
    data class Error(val message: String, val code: Int? = null) : NetworkResult<Nothing>()
    object Loading : NetworkResult<Nothing>()
}

// Uso en repositorio
suspend fun getSongRecommendations(): NetworkResult<List<Song>> {
    return try {
        val response = spotifyApi.getRecommendations()
        NetworkResult.Success(response.tracks.map { it.toDomain() })
    } catch (e: Exception) {
        NetworkResult.Error(e.message ?: "Unknown error")
    }
}
```

### **3. Offline-First con Room + Coroutines**
```kotlin
// data/repository/impl/SongRepositoryImpl.kt
class SongRepositoryImpl(
    private val spotifyApi: SpotifyApi,
    private val songDao: SongDao,
    private val networkChecker: NetworkChecker
) : SongRepository {
    
    override fun getSongs(): Flow<List<Song>> = flow {
        // 1. Emitir datos de Room (caché)
        emit(songDao.getAllSongs().map { it.toDomain() })
        
        // 2. Si hay red, actualizar desde API
        if (networkChecker.isConnected()) {
            try {
                val apiSongs = spotifyApi.getRecommendations()
                songDao.insertAll(apiSongs.map { it.toEntity() })
                emit(songDao.getAllSongs().map { it.toDomain() })
            } catch (e: Exception) {
                // Usuario ya tiene datos de caché, no hay problema
            }
        }
    }
}
```

### **4. Inyección de Dependencias Simple con Hilt**
```kotlin
// Módulo simple para proveer dependencias
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    
    @Provides
    @Singleton
    fun provideSpotifyApi(): SpotifyApi {
        return Retrofit.Builder()
            .baseUrl("https://api.spotify.com/v1/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(SpotifyApi::class.java)
    }
    
    @Provides
    @Singleton
    fun provideSwipeSongUseCase(
        repository: SwipeRepository
    ): SwipeSongUseCase {
        return SwipeSongUseCase(repository)
    }
}
```