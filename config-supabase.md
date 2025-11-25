## 🗃️ Configuración de Supabase

### **1. Setup en Supabase Dashboard**

1. Crear proyecto en https://supabase.com
2. Obtener credenciales:
   - **Project URL**: `https://xxx.supabase.co`
   - **Anon/Public Key**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`
   - **Service Role Key**: (solo para admin, NO en la app)

### **2. Schema de Base de Datos**

```sql
-- Tabla de usuarios (backup de perfil)
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  spotify_id TEXT UNIQUE NOT NULL,
  display_name TEXT,
  email TEXT,
  profile_image_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de swipes (likes/dislikes)
CREATE TABLE swipes (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  spotify_track_id TEXT NOT NULL,
  direction TEXT CHECK (direction IN ('like', 'dislike')),
  swiped_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(user_id, spotify_track_id)
);

-- Tabla de playlists personalizadas
CREATE TABLE playlists (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  spotify_playlist_id TEXT,
  name TEXT NOT NULL,
  description TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Índices para mejorar rendimiento
CREATE INDEX idx_swipes_user_id ON swipes(user_id);
CREATE INDEX idx_playlists_user_id ON playlists(user_id);
```