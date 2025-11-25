# Autenticación

TODO: Documentar el flujo de autenticación con Supabase y OAuth de Spotify.

Este documento describe cómo se maneja la autenticación, utilizando Supabase con OAuth de Spotify.

## Setup Inicial

1. Configuración en Spotify Developer Dashboard:
   - Crear una aplicación y obtener el Client ID y Client Secret.
   - Configurar el Redirect URI (nuestro ejemplo, `songswipe://callback`).
2. Configuración en Supabase:
   - Habilitar el proveedor OAuth de Spotify en la sección de autenticación.
     - Supabase link: https://supabase.com/docs/guides/auth/social-login/auth-spotify?queryGroups=language&language=kotlin&queryGroups=environment&environment=client


**Puntos clave del flujo:**
- Supabase maneja todo el flujo OAuth internamente
- Tokens se almacenan de forma segura automáticamente
- El `provider_token` (token de Spotify) está disponible para llamadas a Spotify API
- Room se usa solo para caché del perfil del usuario
- No se implementa refresh de tokens manualmente
