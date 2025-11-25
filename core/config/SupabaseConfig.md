# SupabaseConfig

## Propósito
Centraliza la configuración específica de Supabase, inicializa el cliente y expone credenciales necesarias para autenticación y acceso a la base de datos.

## Responsabilidades
- Almacenar URL del proyecto y anon key de Supabase
- Inicializar el cliente de Supabase con configuración adecuada
- Configurar opciones de autenticación (persistencia de sesión, auto-refresh de tokens)
- Definir nombres de tablas y schemas utilizados
- Proveer instancia singleton del cliente para uso en toda la app

## Consideraciones
- **Seguridad**: La anon key es segura para cliente, pero configurar RLS (Row Level Security) en Supabase
- Las credenciales deben cargarse desde `local.properties` o variables de entorno, no commitear en Git
- Verificar que la URL incluya el protocolo HTTPS