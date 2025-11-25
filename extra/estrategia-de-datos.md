# 🗄️ Estrategia de Datos: Híbrida (Local-First con Sync)

## **Prioridad 1: Room (Local, Offline-First)** 💾
```kotlin
// Datos críticos guardados localmente
Room Database:
├── UserEntity (perfil, preferencias)
├── SongEntity (canciones vistas, caché)
├── SwipeEntity (likes/dislikes del usuario)
└── PlaylistEntity (playlists creadas)
```

## **Prioridad 2: Supabase (Backup en Cloud)** ☁️
```kotlin
// Sincronización periódica para:
Supabase PostgreSQL:
├── users (backup de perfil)
├── swipes (historial persistente)
└── playlists (backup de playlists)
```

## **Flujo de Sincronización:**
```
1. Usuario hace swipe → Guardar en Room (instantáneo)
2. Background WorkManager → Sync con Supabase cada 15 min
3. App abre → Verificar si hay datos nuevos en Supabase
4. Si hay conexión → Sincronizar; Si no → Usar Room
```
