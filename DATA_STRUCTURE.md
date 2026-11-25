# DATA STRUCTURE - MarketFlow AI

## Purpose

Defines the Supabase (PostgreSQL) database structure that stores products, questions and answers. Designed to scale to multiple countries.

> **Note:** Table and column names are kept in Spanish because they match the live Supabase schema.

---

## Table 1: PRODUCTOS (products)

Basic information for each product you sell.

```sql
CREATE TABLE productos (
  id TEXT PRIMARY KEY,                    -- e.g. "PROD-001"
  nombre VARCHAR(255) NOT NULL,           -- name, e.g. "iPhone 13 Pro"
  descripcion TEXT,                       -- full product description
  precio DECIMAL(12,2) NOT NULL,          -- price, e.g. 3500000.00
  moneda VARCHAR(3) NOT NULL,             -- currency: "COP", "ARS", "MXN", etc.
  stock INT NOT NULL DEFAULT 0,           -- available quantity
  pais VARCHAR(2) NOT NULL,               -- country: "CO", "AR", "MX", etc.
  condicion VARCHAR(50),                  -- condition: "nuevo", "usado", "reacondicionado"
  garantia_meses INT,                     -- warranty in months, e.g. 12
  politica_devolucion TEXT,               -- return policy, e.g. "30 days" or a link
  metodos_pago TEXT[],                    -- payment methods: ["MercadoPago", "Efectivo", "Transferencia"]
  caracteristicas JSONB,                  -- flexible attributes: color, capacity, etc.
  seller_id VARCHAR(100),                 -- seller ID on ML
  creado_en TIMESTAMP DEFAULT NOW(),      -- created at
  actualizado_en TIMESTAMP DEFAULT NOW(), -- updated at
  
  INDEX (pais),
  INDEX (seller_id)
);
```

**Sample data:**
```json
{
  "id": "PROD-001",
  "nombre": "iPhone 13 Pro",
  "descripcion": "Apple smartphone, 6.1-inch display, 128GB",
  "precio": 3500000.00,
  "moneda": "COP",
  "stock": 5,
  "pais": "CO",
  "condicion": "nuevo",
  "garantia_meses": 12,
  "politica_devolucion": "30 days from purchase",
  "metodos_pago": ["MercadoPago", "Efectivo", "Transferencia"],
  "caracteristicas": {
    "color": "Blue",
    "capacidad_gb": 128,
    "modelo": "iPhone 13 Pro",
    "pantalla": "6.1-inch OLED"
  },
  "seller_id": "USER123456"
}
```

---

## Table 2: ENVIOS_X_ZONA (shipping by zone)

Shipping costs and times by region/city. Scalable to multiple countries.

```sql
CREATE TABLE envios_x_zona (
  id SERIAL PRIMARY KEY,
  producto_id TEXT NOT NULL,              -- FK to productos.id
  pais VARCHAR(2) NOT NULL,               -- "CO", "AR", "MX", etc.
  provincia VARCHAR(100),                 -- state/department: "Cundinamarca", "Buenos Aires", etc.
  ciudad VARCHAR(100) NOT NULL,           -- city: "Bogotá", "Medellín", etc.
  costo_envio DECIMAL(10,2) NOT NULL,     -- cost in local currency
  tiempo_entrega_dias VARCHAR(20),        -- delivery days: "1-2", "2-3", "3-4"
  disponible BOOLEAN DEFAULT TRUE,        -- do we ship to this zone?
  creado_en TIMESTAMP DEFAULT NOW(),
  actualizado_en TIMESTAMP DEFAULT NOW(),
  
  FOREIGN KEY (producto_id) REFERENCES productos(id),
  INDEX (producto_id, pais, ciudad),
  UNIQUE (producto_id, pais, ciudad)      -- one cost per product/country/city
);
```

**Sample data (Colombia):**
```
| producto_id | pais | provincia     | ciudad      | costo   | tiempo | disponible |
|-------------|------|---------------|-------------|---------|--------|-----------|
| PROD-001    | CO   | Cundinamarca  | Bogotá      | 30000   | 1-2    | true       |
| PROD-001    | CO   | Antioquia     | Medellín    | 40000   | 2-3    | true       |
| PROD-001    | CO   | Valle         | Cali        | 50000   | 2-3    | true       |
| PROD-001    | CO   | Atlántico     | Barranquilla| 55000   | 2-3    | true       |
| PROD-001    | CO   | Magdalena     | Santa Marta | 70000   | 3-4    | true       |
```

**Future example (Argentina):**
```
| PROD-001    | AR   | Buenos Aires  | Buenos Aires| 2500    | 1-2    | true       |
| PROD-001    | AR   | Córdoba       | Córdoba     | 3500    | 2-3    | true       |
```

---

## Table 3: PREGUNTAS (questions)

Logs every question a customer asks about a product.

```sql
CREATE TABLE preguntas (
  id SERIAL PRIMARY KEY,
  producto_id TEXT NOT NULL,              -- FK to productos.id
  pregunta_texto TEXT NOT NULL,           -- customer's question
  cliente_id VARCHAR(100),                -- customer ID on ML
  cliente_ubicacion VARCHAR(100),         -- customer's city
  pais VARCHAR(2) NOT NULL,               -- country of the question
  canal VARCHAR(50),                      -- channel: "mercado_libre", "ebay", "shopify", etc.
  estado VARCHAR(50) DEFAULT 'pendiente', -- status: "pendiente", "respondida", "rechazada"
  tipo_pregunta VARCHAR(100),             -- type: "disponibilidad", "precio", "envio", etc.
  creado_en TIMESTAMP DEFAULT NOW(),
  
  FOREIGN KEY (producto_id) REFERENCES productos(id),
  INDEX (producto_id, estado, pais),
  INDEX (creado_en)
);
```

**Example:**
```json
{
  "id": 1,
  "producto_id": "PROD-001",
  "pregunta_texto": "¿Cuánto cuesta el envío a Bogotá?",
  "cliente_id": "CLIENTE456",
  "cliente_ubicacion": "Bogotá",
  "pais": "CO",
  "canal": "mercado_libre",
  "estado": "pendiente",
  "tipo_pregunta": "envio",
  "creado_en": "2024-05-14T10:30:00"
}
```

---

## Table 4: RESPUESTAS (answers)

Stores suggested and final answers. Enables history review and prompt improvement.

```sql
CREATE TABLE respuestas (
  id SERIAL PRIMARY KEY,
  pregunta_id INT NOT NULL,               -- FK to preguntas.id
  producto_id TEXT NOT NULL,              -- FK to productos.id
  respuesta_sugerida TEXT NOT NULL,       -- what Claude suggests
  respuesta_final TEXT,                   -- what you sent (if edited)
  estado VARCHAR(50) DEFAULT 'revision',  -- status: "revision", "aceptada", "rechazada"
  accion_usuario VARCHAR(50),             -- user action: "aceptada", "editada", "rechazada"
  feedback_usuario TEXT,                  -- notes on why it was rejected
  generado_por VARCHAR(50) DEFAULT 'claude', -- system that generated the answer
  modelo_ia VARCHAR(100),                 -- AI model, e.g. "claude-haiku-4-5"
  tiempo_respuesta_ms INT,                -- how long Claude took
  confianza FLOAT,                        -- confidence score (0-1), future use
  creado_en TIMESTAMP DEFAULT NOW(),
  actualizado_en TIMESTAMP DEFAULT NOW(),
  
  FOREIGN KEY (pregunta_id) REFERENCES preguntas(id),
  FOREIGN KEY (producto_id) REFERENCES productos(id),
  INDEX (estado, creado_en),
  INDEX (producto_id, estado)
);
```

**Example** (customer-facing text stays in Spanish):
```json
{
  "id": 1,
  "pregunta_id": 1,
  "producto_id": "PROD-001",
  "respuesta_sugerida": "El envío a Bogotá cuesta $30.000 y tarda 1-2 días hábiles. ¿Tienes otras preguntas?",
  "respuesta_final": "El envío a Bogotá cuesta $30.000 y llega en 1-2 días. ¿Puedo ayudarte en algo más?",
  "estado": "aceptada",
  "accion_usuario": "editada",
  "feedback_usuario": "Adjusted wording to sound more natural",
  "generado_por": "claude",
  "modelo_ia": "claude-haiku-4-5-20251001",
  "tiempo_respuesta_ms": 245,
  "confianza": 0.92,
  "creado_en": "2024-05-14T10:31:00"
}
```

---

## Table Relationships

```
productos (1) ──── (N) envios_x_zona
   │
   └──── (N) preguntas
              │
              └──── (1) respuestas
```

---

## Important Notes

1. **Scalability:** The `pais` field allows adding new countries without changing the structure
2. **Indexes:** Optimized for common queries (by country, status, product)
3. **JSONB:** `caracteristicas` is flexible, so fields can be added without migrations
4. **Timestamps:** Every table has `creado_en` and `actualizado_en` for auditing
5. **History:** Stored answers enable analysis and continuous improvement

---

## Next Steps

1. Create these tables in Supabase
2. Insert test data
3. Use Supabase's auto-generated REST API
4. Connect from n8n

---

## Related Documents

- `ARCHITECTURE.md` - System overview
- `PHASE_PLAN.md` - Project phases
