## Notas para Implementación Futura

### Prioridad Alta
- [ ] Configurar Supabase en el proyecto (credenciales, cliente)
- [ ] Implementar `LoginUseCase.kt` con integración de Supabase Auth
- [ ] Crear `AuthRepository.kt` y su implementación con SupabaseClient
- [ ] Configurar inyección de dependencias con Hilt
- [ ] Implementar `SpotifyAuthInterceptor` para inyectar tokens
- [ ] Crear mappers para convertir datos de Supabase y Spotify a entidades de dominio

### Prioridad Media
- [ ] Implementar Room Database (`data/datasource/local/database/AppDatabase.kt`) para caché local
- [ ] Casos de uso de Swipe (`domain/usecase/swipe/SwipeSongUseCase.kt`)
- [ ] ViewModels de Swipe y Playlist en `presentation/viewmodels/`
- [ ] Componentes de UI reutilizables en `presentation/components/`

### Prioridad Baja
- [ ] Estadísticas de usuario (`domain/usecase/user/GetUserStatsUseCase.kt`)
- [ ] Búsqueda avanzada de canciones (`domain/usecase/song/SearchSongsUseCase.kt`)
- [ ] Preferencias personalizadas (`presentation/screens/profile/PreferencesScreen.kt`)


## Flujo de Trabajo Recomendado

### **Fase 1: MVP (2-3 semanas)**
- ✅ Login con Spotify (OAuth)
- ✅ Mostrar recomendaciones de canciones (Spotify API)
- ✅ Swipe like/dislike (guardar en Room)
- ✅ Crear playlist local con likes
- ✅ Persistencia offline (Room)

### **Fase 2: Sync Cloud (1-2 semanas)**
- ✅ Integrar Supabase
- ✅ Sincronizar swipes en background (WorkManager)
- ✅ Backup de playlists en Supabase
- ✅ Sincronización al abrir app

### **Fase 3: Features Avanzadas (2-3 semanas)**
- ✅ Crear playlist directamente en Spotify
- ✅ Historial de swipes
- ✅ Estadísticas de usuario
- ✅ Preferencias personalizadas

### **Fase 4: Polish (1 semana)**
- ✅ Mejorar UI/UX
- ✅ Animaciones con Compose
- ✅ Testing (>70% cobertura)
- ✅ Optimización de rendimiento
