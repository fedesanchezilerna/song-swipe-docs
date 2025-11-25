## ✅ Checklist de Setup Inicial

```bash
# 1. Clonar el repo
git clone https://github.com/ilerna/song-swipe-frontend.git
cd song-swipe-frontend

# 2. Crear archivo de variables de entorno
cp .env.example .env
# Editar .env con tus credenciales de Spotify y Supabase

# 3. Sincronizar Gradle
./gradlew --refresh-dependencies

# 4. Compilar y ejecutar
./gradlew assembleDebug
./gradlew installDebug

# 5. Ejecutar tests
./gradlew test
./gradlew connectedAndroidTest
```

**Archivo `.env.example`:**
```properties
# Spotify Credentials
SPOTIFY_CLIENT_ID=your_client_id_here
SPOTIFY_REDIRECT_URI=songswipe://callback

# Supabase Credentials
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your_anon_key_here
```