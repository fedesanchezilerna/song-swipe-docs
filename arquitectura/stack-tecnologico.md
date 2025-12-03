## Stack Tecnológico

### **UI Layer**
```kotlin
// Jetpack Compose - UI declarativa moderna
implementation("androidx.compose.ui:ui:1.5.4")
implementation("androidx.compose.material3:material3:1.1.2")
implementation("androidx.navigation:navigation-compose:2.7.5")

// Coil - Carga de imágenes optimizada
implementation("io.coil-kt:coil-compose:2.5.0")
```

### **Architecture Components**
```kotlin
// ViewModel y Lifecycle
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.6.2")
implementation("androidx.lifecycle:lifecycle-runtime-compose:2.6.2")

// Hilt - Inyección de dependencias
implementation("com.google.dagger:hilt-android:2.48")
kapt("com.google.dagger:hilt-compiler:2.48")
```

### **Networking**
```kotlin
// Retrofit - Cliente HTTP para Spotify API
implementation("com.squareup.retrofit2:retrofit:2.9.0")
implementation("com.squareup.retrofit2:converter-gson:2.9.0")
implementation("com.squareup.okhttp3:okhttp:4.12.0")
implementation("com.squareup.okhttp3:logging-interceptor:4.12.0")
```

### **Persistencia (Futuro - No en MVP)**
```kotlin
// Room - Base de datos SQLite local (implementar en v2)
// implementation("androidx.room:room-runtime:2.6.1")
// implementation("androidx.room:room-ktx:2.6.1")
// kapt("androidx.room:room-compiler:2.6.1")

// DataStore - Preferencias modernas (implementar en v2)
// implementation("androidx.datastore:datastore-preferences:1.0.0")
```

### **Supabase Client**
```kotlin
// Supabase Auth - Solo autenticación en MVP
implementation("io.github.jan-tennert.supabase:auth-kt:2.0.0")

// Ktor - Requerido por Supabase
implementation("io.ktor:ktor-client-android:2.3.7")
implementation("io.ktor:ktor-client-core:2.3.7")

// Futuro: Postgrest, Realtime, Storage (no en MVP)
// implementation("io.github.jan-tennert.supabase:postgrest-kt:2.0.0")
// implementation("io.github.jan-tennert.supabase:realtime-kt:2.0.0")
// implementation("io.github.jan-tennert.supabase:storage-kt:2.0.0")
```

### **Seguridad**
```kotlin
// Encrypted SharedPreferences - Almacenamiento seguro
implementation("androidx.security:security-crypto:1.1.0-alpha06")
```

### **Background Tasks**
```kotlin
// WorkManager - Sincronización en background
implementation("androidx.work:work-runtime-ktx:2.9.0")
```

### **Testing**
```kotlin
// Unit Testing
testImplementation("junit:junit:4.13.2")
testImplementation("io.mockk:mockk:1.13.8")
testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.3")

// UI Testing
androidTestImplementation("androidx.compose.ui:ui-test-junit4:1.5.4")
androidTestImplementation("androidx.test.ext:junit:1.1.5")
```