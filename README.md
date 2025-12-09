# Song Swipe - Documentación

## Descripcion

Este repositorio contiene la documentación técnica completa del proyecto **Song Swipe**, una aplicación Android que permite descubrir y gestionar música de forma intuitiva mediante swipes, integrándose con Spotify.

## Propósito

La documentación está diseñada para:

- Facilitar la comprensión de la arquitectura y estructura del proyecto
- Servir como referencia técnica para el desarrollo y mantenimiento
- Documentar las decisiones de diseño y patrones implementados
- Proporcionar guías de configuración y autenticación

## Estructura

```
song-swipe-docs/
├── arquitectura/      # Capas y estructura arquitectónica
├── auth/              # Sistema de autenticación
├── config/            # Configuraciones de Android y Supabase
├── core/              # Componentes centrales (config, network, utils)
├── data/              # Capa de datos (datasources, repositories, mappers)
├── di/                # Inyección de dependencias
├── domain/            # Casos de uso
└── extra/             # Estrategias y recursos adicionales
```

## Tecnologías Documentadas

- **Android** - Desarrollo nativo con Kotlin
- **Spotify API** - Integración de servicios musicales
- **Supabase** - Backend y autenticación
- **Clean Architecture** - Patrón arquitectónico
- **Dependency Injection** - Gestión de dependencias

## Convenciones

Cada módulo y componente está documentado en archivos Markdown individuales, organizados según su capa arquitectónica correspondiente.

---

**Proyecto:** Song Swipe Frontend  
**Repositorio:** [song-swipe-frontend](https://github.com/selfishara/song-swipe-frontend)