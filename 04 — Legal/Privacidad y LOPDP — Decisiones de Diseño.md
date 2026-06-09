# Privacidad y LOPDP — Decisiones de Diseño

> Para el Dev / Fundador. Savanhi recopila datos personales de tenderos, consumidores, repartidores y usuarios de marcas. La LOPDP (Ley Orgánica de Protección de Datos Personales, Ecuador) está vigente con régimen sancionatorio desde mayo de 2023. El incumplimiento genera multas de 0.1% a 1% de facturación.
> 

> ⚠️ Este documento establece decisiones de diseño con impacto legal. No es asesoría legal — validar con especialista en LOPDP antes del primer deploy a producción.
> 

---

## Datos Sensibles que Savanhi Procesa

| Actor | Tipo de dato | Sensibilidad | Observación |
| --- | --- | --- | --- |
| Consumidor | Ubicación GPS en tiempo real | 🔴 Alta | Muestra dónde está la persona. Requiere consentimiento explícito antes del modal del sistema. |
| Consumidor | Historial de compras por barrio | 🟡 Media | Permite inferir hábitos, nivel económico, zona de residencia. |
| Consumidor | Teléfono (OTP login) | 🟡 Media | Identificador único. Nunca exponer en responses de API. |
| Tendero | Cédula de identidad | 🔴 Alta | Solo requerido en Fase 2 para SRI. Almacenar cifrado. |
| Tendero | Coordenadas exactas de la tienda | 🟡 Media | Ubica un negocio real en el mapa. Exposición controlada (solo radio de búsqueda). |
| Repartidor | Cédula + datos bancarios | 🔴 Alta | Requerido para pagos formales en Fase 2. Almacenar cifrado, acceso solo admin. |
| Marca | RUC + datos de contacto | 🟡 Media | Datos empresariales. Requieren cláusula de tratamiento en el contrato. |

---

## Principios LOPDP Aplicados a Savanhi

### 1. Consentimiento informado

La app del consumidor DEBE mostrar un texto claro **antes** del modal del sistema para permisos de ubicación y notificaciones. El modal del sistema es irreversible si el usuario lo rechaza.

**Implementación en pantalla de Setup Inicial:**

- Antes de pedir ubicación: *"Para avisarte cuando hay una promo a pasos de tu casa, necesitamos tu ubicación"*
- Antes de pedir notificaciones: *"Así te llegamos cuando hay una oferta en tu barrio"*
- Link visible a Política de Privacidad antes del botón "Listo"

### 2. Finalidad declarada

Solo recopilar datos con propósito declarado. Ningún campo "por las dudas".

| Campo eliminado por LOPDP | Por qué no entra |
| --- | --- |
| Email del consumidor | No se necesita para el modelo. No pedir lo que no se usa. |
| Foto de perfil del consumidor | Innecesario para el negocio. |
| Historial de notificaciones (MVP) | No se usa en MVP. Agregar solo cuando haya UI para mostrarlo. |

### 3. Minimización de datos

La tabla `users` solo almacena los campos estrictamente necesarios para cada rol. Los datos de Fase 2 (cédula, datos bancarios del repartidor) NO se guardan en MVP.

### 4. Seguridad técnica mínima requerida

- [ ]  Cifrado en tránsito: HTTPS/TLS en todos los endpoints (obligatorio desde día 1)
- [ ]  Cifrado en reposo: campos sensibles (cédula, datos bancarios) con `pgcrypto` en PostgreSQL
- [ ]  JWT con expiración corta + refresh tokens
- [ ]  `expo-secure-store` para tokens en React Native (NO AsyncStorage)
- [ ]  Logs de autenticación: `last_login_at` en tabla `users`
- [ ]  Acceso admin restringido: solo el rol ADMIN puede ver datos sensibles de repartidores

### 5. Derechos del titular (obligaciones del sistema)

| Derecho | Cómo Savanhi lo implementa |
| --- | --- |
| Acceso a sus datos | El consumidor puede ver sus datos desde Perfil. El tendero desde su perfil. Ninguno puede ver datos de otros. |
| Rectificación | Editar nombre, horario, foto y método de cobro desde la app. |
| Eliminación | Botón "Eliminar mi cuenta" en configuración. Borra datos personales, anonimiza registros de transacciones (para mantener integridad contable). |
| Portabilidad | No en MVP — evaluar en Fase 2. |
| Oposición a notificaciones | Toggle de notificaciones push en Perfil. Desactivar actualiza `push_enabled = false` en la tabla `users`. |

---

## Decisiones de Diseño con Impacto Legal

### Geolocalización del consumidor

- La ubicación se usa **solo** para Discovery y notificaciones geolocalizadas. No se almacena el historial de posiciones.
- Solo se registra la última posición activa en sesión. Al cerrar la app, no se trackea.
- No se vende ni comparte con terceros (incluidas las marcas) la ubicación individual de consumidores.
- Las marcas ven **mapas de calor agregados** (zona + redenciones), nunca la ubicación de un usuario individual.

### Datos de menores de edad

- La LOPDP tiene controles reforzados para datos de menores (categoría especial).
- Savanhi NO tiene mecanismo de verificación de edad en MVP.
- **Decisión de diseño MVP**: los Términos de Uso deben declarar que el servicio es para mayores de 18 años.
- Fase 2: evaluar verificación de edad en el onboarding.

### Datos de tenderos adultos mayores

- Datos recopilados mediante el Embajador de Barrio (presencial). El consentimiento debe ser verbal y documentado por el Embajador.
- El Embajador debe explicar en lenguaje sencillo qué datos se guardan y para qué.
- Crear un formulario de consentimiento físico simple para el piloto (1 página, lenguaje accesible).

---

## Documentos LOPDP Pendientes de Redactar

> Estos documentos son requerimientos legales de la LOPDP. Deben redactarse **antes** del lanzamiento a producción. Contratar a un especialista en privacidad ecuatoriano.
> 
- [ ]  **Política de Privacidad** — página web + link dentro de la app (onboarding)
- [ ]  **Términos de Uso** — link en pantalla de registro
- [ ]  **Formulario de consentimiento para tenderos** — para el piloto presencial
- [ ]  **Registro de actividades de tratamiento** — obligatorio si se supera el umbral de datos procesados
- [ ]  **Política de retención de datos** — cuánto tiempo se guardan datos de usuarios inactivos
- [ ]  **Procedimiento de atención de derechos ARCO** (Acceso, Rectificación, Cancelación, Oposición)

---

## Multas LOPDP — Régimen Sancionatorio

| Tipo de infracción | Rango de multa |
| --- | --- |
| Infracciones leves | 0.1% a 0.7% del volumen de negocio |
| Infracciones graves | 0.7% a 1% del volumen de negocio |

**Ente regulador:** Superintendencia de Protección de Datos Personales (SPDP)

**Referencias legales:**

- Ley Orgánica de Protección de Datos Personales — Registro Oficial Nº 459, 26 mayo 2021
- Reglamento General LOPDP — Decreto Ejecutivo 904, noviembre 2023
- Régimen sancionatorio vigente desde mayo 2023