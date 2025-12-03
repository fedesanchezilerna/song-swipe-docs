# AppModule

Provee dependencias generales de la aplicación que no pertenecen a categorías específicas (red, base de datos, repositorios).

## Responsabilidades
- Proveer instancia de `Context` de la aplicación
- Proveer `CoroutineScope` para operaciones globales
- Proveer `Dispatchers` (IO, Main, Default) para corrutinas
- Proveer utilidades transversales (loggers, gestores de recursos)

## Anotaciones Hilt
- `@Module`: Marca la clase como módulo de Hilt
- `@InstallIn(SingletonComponent::class)`: Scope de aplicación
- `@Provides`: Métodos que proveen dependencias
- `@Singleton`: Garantiza única instancia en toda la app

## Consideraciones
- Mantener este módulo ligero, solo dependencias verdaderamente globales
- *No incluir lógica de negocio*
- Dependencias específicas van en sus módulos correspondientes