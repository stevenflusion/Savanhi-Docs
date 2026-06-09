# Delivery — Dominio, Tarifas y Operación

> Para el equipo de desarrollo y cofundador de negocios. Define el dominio Delivery como el 9no bounded context de Savanhi, la estructura tarifaria hiperlocal y las reglas operativas del último kilómetro tienda → consumidor.
> 

---

## Por qué Delivery es un dominio separado

El delivery NO es una extensión de `Order`. Es un bounded context independiente porque:

- Tiene su propio ciclo de vida con estados propios
- Involucra un actor nuevo: el **repartidor** (tercero o freelance)
- Tiene lógica de asignación geográfica propia
- Genera una transacción económica separada (pago al repartidor)
- Puede fallar sin que el `Order` falle (el tendero entrega en mano)

> ⚠️ **Distinción crítica**: `Order` registra que el consumidor pidió y el tendero aceptó. `Delivery` registra que ese pedido fue transportado físicamente. Un `Order` puede existir sin `Delivery` — modalidad **retiro en tienda**.
> 

---

## Reglas de Negocio Críticas

1. **Delivery es opt-in por tienda** — el tendero activa/desactiva en su perfil. No es obligatorio.
2. **Solo tiendas Gold y Silver pueden ofrecer delivery** — Bronze no. Misma lógica que campañas.
3. **Radio máximo: 5 km** — más allá no es hiperlocal y deja de ser ventaja competitiva.
4. **Tiempo máximo de asignación: 3 minutos** — si no hay repartidor disponible, el sistema notifica al consumidor y ofrece retiro en tienda.
5. **El delivery no reemplaza el cupón** — una redención de cupón puede ocurrir en delivery. El tendero escanea antes de entregar al repartidor.
6. **Pago al repartidor via DeUna o cuenta Banco Pichincha** — el repartidor elige su método al registrarse. MVP: el pago lo gestiona el admin manualmente post-entrega. Fase 2: automatizado.
7. **El consumidor NO puede pagar en efectivo en delivery** — solo DeUna. El efectivo queda para retiro en tienda.

---

## Tabla de Tarifas — Modelo Hiperlocal (0–5 km)

### Supuestos base

- **Tarifa al repartidor:** $0.45/km (estándar) | $0.60/km (nocturno/lluvia/alta demanda)
- **Mínimo garantizado por viaje al repartidor:** $1.00
- **Tarifa base al consumidor:** $1.50 (incluye gestión, asignación y primer km)
- **Por km adicional al consumidor:** $0.50/km (km 2 en adelante)

### Tabla estándar ($0.45/km al repartidor)

| Distancia | Consumidor paga | Repartidor recibe | Margen Savanhi |
| --- | --- | --- | --- |
| 0–1 km | **$1.50** | $1.00 (mín. garantizado) | **$0.50** |
| ~2 km | **$2.00** | $1.00 (mín. aplica) | **$1.00** |
| ~3 km | **$2.50** | $1.35 | **$1.15** |
| ~4 km | **$3.00** | $1.80 | **$1.20** |
| ~5 km | **$3.50** | $2.25 | **$1.25** |

### Tabla premium ($0.60/km — nocturno / lluvia / alta demanda)

| Distancia | Consumidor paga | Repartidor recibe | Margen Savanhi |
| --- | --- | --- | --- |
| 0–1 km | **$1.80** | $1.00 (mín. garantizado) | **$0.80** |
| ~2 km | **$2.30** | $1.20 | **$1.10** |
| ~3 km | **$2.80** | $1.80 | **$1.00** |
| ~4 km | **$3.30** | $2.40 | **$0.90** |
| ~5 km | **$3.80** | $3.00 | **$0.80** |

> ⚠️ **Pendiente de validación**: Los valores $0.45 y $0.60/km son referencias de campo. Validar contra al menos 3 repartidores reales en Calderón durante el field research (semanas 1–2).
> 

### Fórmula de cálculo

```
// Lo que paga el consumidor
consumerPrice = 1.50 + max(0, distanceKm - 1) * 0.50

// Lo que recibe el repartidor (tarifa estándar)
driverPayout = max(1.00, distanceKm * 0.45)

// Lo que recibe el repartidor (tarifa premium)
driverPayoutPremium = max(1.00, distanceKm * 0.60)

// Margen Savanhi
margin = consumerPrice - driverPayout
```

### Proyección de ingreso por delivery

Asumiendo distancia promedio de 2 km y tarifa estándar:

- Por delivery: **$1.00 de margen**
- 100 deliveries/semana: **$100/semana** → $400/mes
- 500 deliveries/semana (escala): **$2,000/mes** solo por delivery

---

## Estados y Transiciones

| Estado | Descripción | Quién dispara |
| --- | --- | --- |
| `PENDING` | Creado, buscando repartidor | Sistema automático |
| `ASSIGNED` | Repartidor aceptó el viaje | Driver app |
| `PICKED_UP` | Repartidor recogió en tienda | Driver escanea / confirma |
| `IN_TRANSIT` | En camino al consumidor | Automático al PICKED_UP |
| `DELIVERED` | Entregado al consumidor | Driver confirma + foto opcional |
| `FAILED` | No se pudo entregar | Driver reporta |
| `CANCELLED` | Cancelado antes de PICKED_UP | Consumidor o sistema |

---

## Alcance MVP vs Fase 2

| Feature | MVP | Fase 2 |
| --- | --- | --- |
| Asignación manual de repartidor | ✅ Tendero asigna repartidor conocido | — |
| Asignación automática (más cercano) | ❌ | ✅ Requiere pool de drivers activos |
| Tracking en tiempo real (mapa consumidor) | ❌ | ✅ WebSocket + Mapbox |
| Driver app propia | ❌ MVP usa WhatsApp/llamada | ✅ App dedicada Fase 2 |
| Pago al repartidor via DeUna | ✅ Post-entrega inmediato (manual admin) | ✅ Automatizado via webhook |
| Tarifa dinámica (demanda/lluvia) | ❌ | ✅ Algoritmo de surge |
| Rating al repartidor | ❌ | ✅ Post Review |

---

## Riesgos Operativos

| Riesgo | Mitigación |
| --- | --- |
| No hay repartidores disponibles en el barrio piloto | MVP: tendero usa su propio repartidor (modelo híbrido). Agregar pool en Fase 2. |
| Repartidor no llega / fraude de entrega | Foto de entrega requerida. Rating post-delivery. Pago DeUna post-confirmación del consumidor. |
| Tarifa $0.45/km no es rentable para el repartidor | Validar en campo semanas 1–2. El mínimo garantizado de $1.00 protege viajes cortos. |
| Consumidor no está en casa | Máx 2 intentos de entrega. Al 3er intento → FAILED → reembolso o retiro en tienda. |

---

## Decisiones Pendientes

| Decisión | Responsable | Estado |
| --- | --- | --- |
| Validar $0.45 y $0.60/km con repartidores reales en Calderón | Cofundador negocios | ⏳ Field research semanas 1–2 |
| ¿Savanhi contrata repartidores o son freelance? | Cofundador negocios + finanzas | ⏳ Pendiente |
| Obligaciones SRI por pago a repartidores independientes | Cofundador finanzas | ⏳ Pendiente |
| Tarifa premium: ¿horario nocturno desde qué hora? | Cofundador negocios | ⏳ Pendiente |
| ¿Radio máximo 5km o reducir a 3km para piloto? | Cofundador negocios | ⏳ Pendiente |
| Seguro de responsabilidad civil para repartidores | Cofundador finanzas | ⏳ Pendiente |