# Marketing y Producto

> Documento de decisiones de **producto, UX y go-to-market**.
> 

> Info de software ignorada según request del usuario.
> 

---

# Los 4 Pilares del Producto

> **Sin promociones, Savanhi es conveniente. Con promociones, Savanhi es irresistible. Con delivery, Savanhi es imbatible.**
> 
1. **Descubrimiento hiperlocal** — el consumidor encuentra el producto en su barrio
2. **Motor de promociones** — ofertas reales de marcas en tiendas cercanas
3. **Pagos digitales** — DeUna como vía principal, efectivo como fallback
4. **Delivery hiperlocal** — el pedido llega a la puerta del consumidor sin salir del barrio

---

# DeUna — Estado Actualizado 2026

| Dato | Valor | Fuente |
| --- | --- | --- |
| Usuarios activos DeUna | +6 millones | Banco Pichincha (2026) |
| Comercios que cobran via DeUna | +620.000 | Banco Pichincha (2026) |
| Comercios con Deuna Negocios | +35.000 | Deuna (abril 2026) |
| Crecimiento transacciones 2024-2025 | 5x | Forbes Ecuador |
| DeUna funciona sin internet | ✅ Sí (oficial 2025) | Banco Pichincha |

> **Implicación de diseño**: DeUna es la vía principal para pagos digitales, pero el efectivo nunca puede eliminarse del MVP. La verificación de cupones debe funcionar de forma independiente al pago.
> 

---

# Brecha de Adopción Digital — Tendero Adulto Mayor

Una fracción de tenderos — especialmente adultos mayores — no usa DeUna ni tiene familiaridad con apps móviles. Este segmento es crítico:

1. Son tenderos con años de permanencia — tienen la confianza más alta del consumidor
2. Sin adopción de ellos, el mapa de tiendas tiene huecos en barrios clave
3. No es un problema técnico — es un problema de onboarding presencial

> 🔴 **ANALIZAR CON CO-FUNDADOR DE NEGOCIOS**: quién ejecuta el onboarding presencial de estos tenderos, cuánto cuesta por tienda, y si el Embajador de Barrio es suficiente.
> 

## Opciones de mitigación

| Estrategia | Cómo funciona | Caso referente | Costo estimado |
| --- | --- | --- | --- |
| **Embajador de Barrio** ← recomendada MVP | 1 persona por barrio hace el onboarding presencial | Wabi (Perú), Frubana (Colombia) | $500–700/mes por embajador |
| **Tendero-agente** | Tendero Gold actúa como onboarder de tiendas vecinas | Chiper (Colombia) | 0 costo fijo |
| **Formulario físico + carga posterior** | Campo llena formulario, backend sube el catálogo | Frubana en fase inicial | Bajo costo pero lento |

---

# Flujo del Consumidor — Motor de Promociones

1. Recibe notificación push geolocalizada
2. Abre la app → activa el cupón
3. Va a la tienda → muestra el cupón al tendero
4. Tendero escanea el código → sistema marca REDIMIDO

**Reglas de las notificaciones:**

- Solo llegan a usuarios dentro del radio de las tiendas participantes
- Máximo **3 notificaciones por usuario por día**
- Urgencia real: stock limitado, tiempo limitado
- Mensajes en español local — tono directo, no corporativo

---

# Sistema de Verificación de Cupones

1. Consumidor activa cupón en la app
2. Se genera código único irrepetible
3. Tendero escanea o ingresa el código
4. Sistema marca el cupón como REDIMIDO
5. Redención aparece en el panel de la marca en tiempo real
6. Crédito registrado automáticamente al tendero
7. Liquidación semanal al tendero via DeUna

> El código no se puede reutilizar. Intento de reutilización → rechazo instantáneo.
> 

---

# Flujo del Consumidor — Delivery

> El delivery es **opt-in** — el consumidor elige DELIVERY o RETIRO en cada pedido.
> 
1. Consumidor agrega productos al carrito
2. En checkout elige modalidad: **Retiro en tienda** | **Delivery a domicilio**
3. Sistema calcula la tarifa en tiempo real según distancia
4. Consumidor ve el costo de envío y confirma el pedido
5. Pago completo via DeUna (no se acepta efectivo en delivery)
6. Tendero recibe la orden + notificación de modalidad DELIVERY
7. Tendero prepara el pedido y llama a su repartidor del barrio (MVP manual)
8. Repartidor entrega → toma foto de confirmación
9. Sistema marca ENTREGADO → pago inmediato al repartidor via DeUna

## Tarifas visibles al consumidor

| Distancia a la tienda | Costo de envío | Tiempo estimado |
| --- | --- | --- |
| 0-1 km | $1.50 | 10-15 min |
| ~2 km | $2.00 | 15-20 min |
| ~3 km | $2.50 | 20-25 min |
| ~4 km | $3.00 | 25-30 min |
| ~5 km | $3.50 | 30-40 min |

> Tarifa nocturna/lluvia/alta demanda: $1.80 base (0–1 km), +$0.50/km adicional.
> 

---

# Panel de Marca — Datos en Tiempo Real

## Datos de campaña

| Métrica | Descripción |
| --- | --- |
| Cupones activados | Cuántos usuarios activaron el cupón |
| Tasa de activación | Activados / Notificaciones enviadas |
| Cupones redimidos | Canjeados físicamente en tienda |
| Tasa de redención | Redimidos / Activados — el KPI principal |
| Presupuesto ejecutado | Cuánto se ha gastado del total asignado |

## Datos geográficos e hiperlocales

| Métrica | Descripción |
| --- | --- |
| Mapa de calor de redenciones | Qué barrios de Quito convierten más |
| Ranking de tiendas por redención | Top 10 tiendas de la campaña |
| Franjas horarias de conversión | A qué hora se redimen más cupones |

---

# Gamificación — Dashboard del Tendero

- **Barra de progreso Gold:** *"Estás a 8 puntos de Gold. Las tiendas Gold reciben 3 campañas/mes en promedio."*
- **Diagnóstico Bronze:** *"Tu tasa de aceptación bajó a 62%. Las tiendas similares tienen 85%."*
- **Reto 30 días** con temporizador visible

> El tier Gold/Silver/Bronze es **interno** — las marcas lo ven en su panel. El consumidor ve estrellas (1–5), no el nivel.
> 

## Sistema de Tier

| Nivel | Requisitos | Beneficios |
| --- | --- | --- |
| **Gold** | ≥ 15 cupones/semana, ≥ 80% tasa de aceptación | 3+ campañas/mes, prioritization |
| **Bronze** | < 8 cupones/semana, < 60% tasa de aceptación | Solo retiro en tienda |

---

# Criterios de Selección — Tiendas de Condominio (Actualizado mayo 2026)

> **Pivote mayo 2026:** Se reemplaza el modelo de "barrios abiertos" por **tiendas de condominio o tiendas adyacentes a conjuntos residenciales**. Razón: clientela cautiva de mínimo 20 usuarios por tienda, el tendero es el canal de adquisición, no Savanhi.
> 

| Criterio | Tienda de condominio / conjunto residencial |
| --- | --- |
| Tipo de ubicación | Dentro del conjunto o en la entrada inmediata (&lt;100m) |
| Clientes mínimos garantizados | ≥ 20 familias/unidades del condominio |
| NSE objetivo | C/D (conjuntos de clase media-baja, no urbanizaciones premium) |
| Relación tendero–cliente | Conocen al tendero por nombre — alta confianza preexistente |
| Adopción de DeUna | ≥ 30% de tiendas objetivo con QR DeUna activo |
| Competencia directa (TuTi) | Sin TuTi dentro del conjunto — no hay hard discount dentro de condominios |

---

# Encuesta de Campo — Antes de Construir el MVP

1. ¿El tendero está dispuesto a registrar su catálogo en una app o lo ve como trabajo extra?
2. ¿El consumidor quiteño prefiere recoger el pedido o recibirlo en casa?
3. ¿El tendero participaría en campañas de marcas sin costo propio?
4. ¿Le genera desconfianza el reembolso semanal?
5. ¿Qué tan extendido está DeUna en las tiendas específicas del barrio candidato?
6. ¿Qué le quitaría el sueño hoy: TuTi, inseguridad o deudas?
7. ¿El tendero tiene smartphone? ¿Android o iPhone?

---

# Playbook Operacional — Piloto (1–2 Meses)

## Equipo mínimo viable

| Rol | Responsabilidad principal | Dedicación |
| --- | --- | --- |
| **Fundador / Tech Lead** | MVP, producto, decisiones técnicas | Full-time |
| **Co-Fundador de Negocios** | Contratos con marcas, supervisión campo | Full-time |
| **Embajador de Barrio** (1 por barrio) | Onboarding presencial de tenderos | Full-time por barrio |
| **Co-Fundador de Finanzas** | Reembolsos, SRI, contratos financieros | Part-time durante el piloto |

## Semana a semana — Piloto

| Semana | Acción | Responsable | Criterio de avance |
| --- | --- | --- | --- |
| 1–2 | Field research: visitar 15+ tiendas con encuesta | Co-Fundador Negocios + Steven | Barrios definidos |
| 3–4 | Primer contacto con marca ancla | Co-Fundador Negocios | Reunión agendada |
| 5–6 | Piloto papel: probar flujo de cupón manualmente | Steven + Co-Fundador Negocios | Flujo validado |
| 7–10 | Desarrollo MVP: catalog + discovery + cupón básico | Steven | MVP funcionando en staging |
| 11–12 | Onboarding de tiendas en campo | Embajador de Barrio | ≥ 80% tiendas onboardeadas |
| 13–14 | Primera campaña de marca ancla activa | Co-Fundador Negocios | Primer cupón redimido |
| 15–16 | Medición completa + primer reporte ejecutivo | Steven + Co-Fundador Negocios | Reporte entregado |
| 17+ | Escalar al segundo barrio + segunda marca | Equipo completo | Métricas de éxito alcanzadas |

---

# Panorama Competitivo Actualizado (abr 2026)

| Competidor | Tiendas | Presencia | Amenaza Savanhi |
| --- | --- | --- | --- |
| **TuTi** | **762** | 99+ ciudades, 88% hogares | **ALTA** |
| **B-Sí** | **2** (piloto) | Quito norte | **BAJA** |
| **Tía Go** | **114** | 22 provincias | **MEDIA** |
| **Coralito** | piloto | Cuenca → Quito | **BAJA** |

**La barrera real NO es vs. TuTi/B-Sí — es ser EL PRIMERO en plataforma digital de canal tradicional en Ecuador.**

---

*Documento organizado en SA/02 — Marketing por co-ceo. Abril 2026.*