# 📚 Guía Completa de ECC (Everything Claude Code) en Español

> **Preguntas, respuestas y casos de uso reales**
> 
> Documento compilado con todas tus consultas sobre ECC, Prisma + Redis y cómo implementarlas en proyectos reales.

---

## 📖 Tabla de Contenidos

1. [¿Qué es ECC?](#qué-es-ecc)
2. [Instalación en OpenCode](#instalación-en-opencode)
3. [Cómo se usa ECC](#cómo-se-usa-ecc)
4. [Caso de Uso: Crear un Backend REST](#caso-de-uso-crear-un-backend-rest)
5. [Prisma + Redis: ¿Sirve ECC?](#prisma--redis-sirve-ecc)
6. [Caso Real: E-commerce con Caching](#caso-real-sistema-de-e-commerce)
7. [Recursos y Enlaces](#recursos-y-enlaces)

---

## 🎯 ¿Qué es ECC?

### Pregunta Original
> ¿Qué hace este repo, como se instala en OpenCode, como se usa, dame un caso de uso real en creación de un backend o frontend?

### Respuesta

**ECC** es un **sistema operativo para agentes de IA** que crea un entorno estructurado y profesional para que herramientas como Claude Code, Copilot, Cursor y otros generen código de alta calidad. No es un software que se "instale" en OpenCode, sino un **conjunto de reglas, agentes, habilidades y flujos de trabajo** que mejoran dramáticamente cómo los agentes de IA planifican, implementan, prueban y revisan código.

### Características Principales

- **68 agentes especializados** (planner, reviewer, security auditor, etc.)
- **291 habilidades** para TDD, seguridad, frontend, backend, ML, DevOps
- **Hooks y memoria** para aprendizaje continuo
- **Reglas según lenguaje/framework** (TypeScript, Python, Rust, etc.)
- **AgentShield** - escaneo de seguridad integrado
- **255K+ stars en GitHub** — es altamente confiable

### Stack Tecnológico

```
- JavaScript: 66.1%
- Rust: 17.9%
- Python: 12.8%
- Shell: 2.1%
- TypeScript: 1%
- Swift: 0.1%
```

### Proyecto Original
- **GitHub:** https://github.com/affaan-m/ECC
- **Website:** https://ecc.tools
- **License:** MIT
- **Stars:** 255,915+

---

## 📦 Instalación en OpenCode

### Opción 1: Instalación Guiada Universal (RECOMENDADO)

```bash
npx ecc-universal@2.2.1 setup
```

Elige "OpenCode" cuando pregunte por el harness (entorno).

### Opción 2: Instalación Manual para OpenCode

```bash
git clone https://github.com/affaan-m/ECC.git
cd ECC
npm install && npm run build:opencode
./install.sh --profile full --target opencode
```

### Opción 3: Alternativas por Herramienta

| Harness | Comando |
|---------|----------|
| Claude Code | `npx ecc-universal@2.2.1 setup` |
| Codex | `codex plugin marketplace add affaan-m/ECC` |
| Cursor | `./install.sh --profile minimal --target cursor` |
| Gemini | `./install.sh --profile minimal --target gemini` |
| Zed | `./install.sh --profile minimal --target zed` |

### Resultado de la Instalación

Se crea una carpeta `.opencode/` (o similar) en tu proyecto con:
- Agentes predefinidos
- Habilidades (skills)
- Comandos (prompts reutilizables)
- Reglas de código

---

## 🚀 Cómo se usa ECC

### Flujo de Trabajo Típico

```
plan -> test -> implement -> review -> verify -> remember -> improve
```

### Ejemplo: Crear un Usuario en Backend

```
1. Planificación    → Agent "planner" analiza requisitos
2. Tests            → Agent escribe tests antes de código (TDD)
3. Implementación   → Agent escribe código mínimo para pasar tests
4. Review          → Agent revisa desde contexto fresco
5. Verificación    → Coverage, seguridad, regresos
6. Memoria         → Se recuerda cómo se hizo para próximos proyectos
```

### Comandos en OpenCode

```
/plan crear endpoint POST /users con validación
/tdd usuario con email duplicado falla
/security-review datos de usuario en la API
/code-review
/build-fix
/refactor-clean
```

### Habilidades Incluidas

| Habilidad | Caso de uso |
|-----------|----------|
| `tdd-workflow` | Escribe tests antes de código |
| `backend-patterns` | Arquitectura REST/GraphQL |
| `security-review` | Auditoría de vulnerabilidades |
| `react-patterns` | Frontend con best practices |
| `api-design` | Diseño de APIs RESTful |
| `django-tdd` / `springboot-tdd` | TDD para otros stacks |
| `testing` | Unit, integration, E2E |
| `prisma-patterns` | Queries, transacciones, migrations |
| `redis-patterns` | Caching, locks, rate limiting |

---

## 💼 Caso de Uso: Crear un Backend REST (Node.js + Express)

### Pregunta Original
> ¿Cuál es un caso de uso real en creación de un backend con ECC?

### Requisito
API de productos con autenticación

### Paso 1: Planificación

```
El agente "planner" genera:
├── Phase 1: Modelos de datos (Product, User)
├── Phase 2: Autenticación (JWT + validación)
├── Phase 3: Endpoints CRUD + seguridad
├── Phase 4: Tests (80%+ coverage)
└── Riesgos identificados (inyección SQL, tokens expirados)
```

### Paso 2: Test-Driven Development (TDD)

```typescript
// tests/product.test.js
describe('POST /products', () => {
  it('debería crear producto si estás autenticado', async () => {
    const res = await request(app)
      .post('/products')
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'Laptop', price: 1200 });
    
    expect(res.status).toBe(201);
    expect(res.body.id).toBeDefined();
  });

  it('debería rechazar si no hay token', async () => {
    const res = await request(app)
      .post('/products')
      .send({ name: 'Laptop', price: 1200 });
    
    expect(res.status).toBe(401);
  });
});
```

### Paso 3: Implementación Mínima

```javascript
// src/routes/products.js
app.post('/products', auth, validate(productSchema), async (req, res) => {
  const product = await Product.create(req.body);
  res.status(201).json(product);
});
```

### Paso 4: Revisión de Seguridad

- ✅ Validación de entrada (schema)
- ✅ Autenticación en lado servidor
- ✅ Queries parametrizadas (sin SQL injection)
- ✅ Rate limiting en endpoints públicos
- ✅ Tokens JWT con expiración

### Paso 5: Frontend correspondiente (React)

```javascript
// components/ProductForm.tsx
export function ProductForm() {
  const [products, setProducts] = useState([]);
  
  const handleCreate = async (data) => {
    const res = await fetch('/api/products', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${localStorage.token}`
      },
      body: JSON.stringify(data)
    });
    
    if (res.ok) {
      setProducts([...products, await res.json()]);
    }
  };
  
  return (
    <form onSubmit={(e) => {
      e.preventDefault();
      handleCreate(new FormData(e.target));
    }}>
      <input name="name" required />
      <input name="price" type="number" required />
      <button>Crear</button>
    </form>
  );
}
```

### Paso 6: Resultados

- ✅ API funcionando con autenticación
- ✅ Tests verdes (80%+ coverage)
- ✅ Vulnerabilidades identificadas y mitigadas
- ✅ Documentación automática
- ✅ Preparado para producción

### Flujo Completo con ECC

```
1. /ecc:plan "describe the feature"
   ↓ Planner agent crea plan estructurado
   
2. tdd-workflow
   ↓ TDD agent ejecuta: RED -> GREEN -> REFACTOR
   
3. Implementar código
   ↓ Código mínimo para pasar tests
   
4. /code-review
   ↓ Code-reviewer agent revisa desde contexto fresco
   
5. /security-scan
   ↓ Security-reviewer busca vulnerabilidades
   
6. /build-fix (si hay errores)
   ↓ Build-error-resolver diagnostica
   
7. API lista para producción
```

---

## 🔄 Prisma + Redis: ¿Sirve ECC?

### Pregunta Original
> Si yo uso Prisma como ORM en la base de datos y Redis para el caching, ¿este repo sirve? ¿Qué haría y cómo se lo pediría caso de uso real?

### Respuesta Corta

**Rotundamente SÍ.** ECC tiene habilidades específicas para ambas tecnologías.

### Habilidades Disponibles

| Habilidad | Propósito |
|-----------|----------|
| **prisma-patterns** | Queries, transacciones, migrations, serverless, N+1 prevention |
| **redis-patterns** | Cache-aside, write-through, distributed locks, rate limiting |
| **backend-patterns** | Combinación de ambas en arquitectura real |
| **api-design** | REST design con caching strategies |
| **database-migrations** | Prisma migrations en producción |

### Qué Cubre Prisma Patterns

#### Anti-Patrones Comunes (EVITA)
```typescript
// ❌ BAD: updateMany devuelve count, no records
const users = await prisma.user.updateMany({ 
  where: { role: 'GUEST' }, 
  data: { role: 'USER' } 
});
// users es { count: 2 } — ¡no es un array!

// ✅ GOOD: capturar IDs primero
const targets = await prisma.user.findMany({
  where: { role: 'GUEST' },
  select: { id: true },
});
const ids = targets.map((u) => u.id);
await prisma.user.updateMany({ 
  where: { id: { in: ids } }, 
  data: { role: 'USER' } 
});
const updated = await prisma.user.findMany({ 
  where: { id: { in: ids } } 
});
```

```typescript
// ❌ BAD: soft delete + findUniqueOrThrow filtra mal
const user = await prisma.user.findUniqueOrThrow({ 
  where: { id } 
}); // Devuelve usuarios borrados!

// ✅ GOOD: usar findFirstOrThrow
const user = await prisma.user.findFirstOrThrow({ 
  where: { id, deletedAt: null } 
});
```

```typescript
// ❌ BAD: @updatedAt no se actualiza en updateMany
await prisma.post.updateMany({ 
  where: { authorId }, 
  data: { published: true } 
}); // updatedAt sigue siendo antiguo

// ✅ GOOD: fijar manualmente
await prisma.post.updateMany({
  where: { authorId },
  data: { published: true, updatedAt: new Date() },
});
```

#### Best Practices

| Regla | Razón |
|-------|-------|
| `migrate deploy` en CI/CD, `migrate dev` solo localmente | `migrate dev` puede resetear la BD |
| Mapear entidades a DTOs | Evita exponer campos internos |
| Catch `PrismaClientKnownRequestError` en service layer | Traduce a domain errors |
| Preferir `*OrThrow` sobre null checks | Lanza P2025 automáticamente |
| `connection_limit=1` + external pooler en serverless | Evita agotamiento de conexiones |
| Siempre `WHERE` en `deleteMany` | Previene borrar toda la tabla |
| TTL siempre en Redis keys | Memoria no crece infinitamente |

### Qué Cubre Redis Patterns

#### Estrategias de Caché

```python
# Cache-Aside (Lazy Loading) - MÁS COMÚN
def get_product(product_id: int):
    cache_key = f"product:{product_id}"
    cached = r.get(cache_key)

    if cached:
        return json.loads(cached)

    product = db.query("SELECT * FROM products WHERE id = %s", product_id)
    r.setex(cache_key, 3600, json.dumps(product))  # TTL: 1 hora
    return product

# Write-Through Cache - CONSISTENCIA FUERTE
def update_product(product_id: int, data: dict):
    # Escribir a BD primero
    db.execute("UPDATE products SET ... WHERE id = %s", product_id)

    # Actualizar cache inmediatamente
    cache_key = f"product:{product_id}"
    r.setex(cache_key, 3600, json.dumps(data))
```

#### Invalidación por Tags

```python
# Group related keys under a set
def cache_product(product_id: int, category_id: int, data: dict):
    key = f"product:{product_id}"
    tag = f"tag:category:{category_id}"
    pipe = r.pipeline(transaction=True)
    pipe.setex(key, 3600, json.dumps(data))
    pipe.sadd(tag, key)
    pipe.expire(tag, 3600)
    pipe.execute()

def invalidate_category(category_id: int):
    tag = f"tag:category:{category_id}"
    keys = r.smembers(tag)
    if keys:
        r.delete(*keys)
    r.delete(tag)
```

#### Anti-Patrones a Evitar

| Anti-Patrón | Problema | Solución |
|---|---|---|
| Keys sin TTL | Memoria crece sin límite | Siempre usar `setex` |
| `KEYS *` en producción | Bloquea el servidor (O(N)) | Usar `SCAN` cursor |
| Blobs grandes (>100KB) | Serialización lenta | Guardar referencia |
| Redis para todo | Sin aislamiento entre cache y queue | Usar DBs o instancias separadas |
| Ignorar connection pool | Agotamiento bajo carga | Dimensionar al workload |
| Cache miss stampede | Thundering herd en cold start | Usar locks o early expiry |

#### Quick Reference

| Patrón | Cuándo Usar |
|--------|----------|
| Cache-aside | Read-heavy, tolera staleness |
| Write-through | Consistencia fuerte requerida |
| Distributed lock | Prevenir acceso concurrente |
| Sliding window rate limit | Throttling preciso por usuario |
| Redis Streams | Cola durable con consumer groups |
| Pub/Sub | Broadcast sin garantías |
| Sorted Set | Leaderboards, ranking |
| HyperLogLog | Unique count aproximado |

---

## 🎯 Caso Real: Sistema de E-commerce con Caching

### Pregunta Original
> ¿Cómo usaría ECC en un caso real con Prisma + Redis? ¿Qué haría y cómo se lo pediría?

### Requisito
- Usuarios ven **catálogo de productos** (lectura frecuente)
- Agregan productos al **carrito** (escritura frecuente)
- Ver **productos populares** (lectura pesada, se cachea)
- Todo con **Prisma + Redis** en Express

---

### Paso 1: Planificación con ECC

**Comando exacto:**

```markdown
/ecc:plan Crear sistema de e-commerce con:

**Requisitos:**
- Productos: CRUD con caché 5min
- Carrito: agregar/remover items, invalidar cache automático
- Productos trending: top 10, caché 30min
- Stock: evitar overselling con transacciones
- Tests: 80%+ coverage
- Validación de entrada y seguridad

**Stack:**
- Prisma ORM + PostgreSQL
- Redis para caching
- Express.js
- TypeScript

**Restricciones:**
- Cache-aside pattern obligatorio
- Transacciones Prisma para stock
- Validar N+1 queries
```

**Respuesta del Planner:**
- Phase 1: Schema Prisma + Redis setup
- Phase 2: Endpoints básicos
- Phase 3: Caching strategy
- Phase 4: Tests + seguridad
- Identificación de riesgos (cache stampede, race conditions)

---

### Paso 2: Tests con TDD

**Comando:**
```
/tdd endpoints de producto con caching en Redis
```

```typescript
// tests/products.test.ts
import request from 'supertest';
import app from '../src/app';
import { prisma } from '../src/lib/prisma';
import { redis } from '../src/lib/redis';

describe('GET /products/:id', () => {
  beforeEach(async () => {
    await redis.flushdb();
  });

  it('debería cachear el producto en Redis después de la primera lectura', async () => {
    const product = await prisma.product.create({
      data: { name: 'Laptop', price: 1200, stock: 5 }
    });

    // Primera llamada - cache miss
    const res1 = await request(app).get(`/products/${product.id}`);
    expect(res1.status).toBe(200);
    expect(res1.body.name).toBe('Laptop');

    // Verificar que está en Redis
    const cached = await redis.get(`product:${product.id}`);
    expect(cached).toBeDefined();

    // Segunda llamada - debe venir de Redis (más rápido)
    const res2 = await request(app).get(`/products/${product.id}`);
    expect(res2.status).toBe(200);
    expect(res2.body.name).toBe('Laptop');
  });

  it('debería invalidar cache si el stock cambia', async () => {
    const product = await prisma.product.create({
      data: { name: 'Monitor', price: 300, stock: 10 }
    });

    // Primer GET - cachea
    await request(app).get(`/products/${product.id}`);
    let cached = await redis.get(`product:${product.id}`);
    expect(cached).toBeDefined();

    // Actualizar stock (comprar una unidad)
    await request(app)
      .post(`/cart`)
      .send({ userId: 'user1', productId: product.id, quantity: 1 });

    // Cache debe estar invalidado
    cached = await redis.get(`product:${product.id}`);
    expect(cached).toBeNull();
  });

  it('debería rechazar si no hay stock', async () => {
    const product = await prisma.product.create({
      data: { name: 'GPU', price: 2000, stock: 0 }
    });

    const res = await request(app)
      .post('/cart')
      .send({ userId: 'user1', productId: product.id, quantity: 1 });

    expect(res.status).toBe(400);
    expect(res.body.error).toContain('stock');
  });
});
```

---

### Paso 3: Implementación

#### Setup Prisma

```typescript
// src/lib/prisma.ts
import { PrismaClient } from '@prisma/client';
import { PrismaPg } from '@prisma/adapter-pg';

function createPrismaClient() {
  const adapter = new PrismaPg({
    connectionString: process.env.DATABASE_URL!,
  });
  return new PrismaClient({
    adapter,
    log: process.env.NODE_ENV === 'development' ? ['query', 'error'] : ['error'],
  });
}

const globalForPrisma = globalThis as unknown as { prisma?: PrismaClient };
export const prisma = globalForPrisma.prisma ?? createPrismaClient();
if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```

#### Setup Redis

```typescript
// src/lib/redis.ts
import Redis from 'ioredis';

const redis = new Redis({
  host: process.env.REDIS_HOST || 'localhost',
  port: parseInt(process.env.REDIS_PORT || '6379'),
  retryStrategy: (times) => Math.min(times * 50, 2000),
});

redis.on('error', (err) => console.error('Redis error:', err));
redis.on('connect', () => console.log('Redis connected'));

export { redis };
```

#### Schema Prisma

```prisma
// prisma/schema.prisma
model Product {
  id        String   @id @default(cuid())
  name      String
  price     Float
  stock     Int      @default(0)
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  cartItems CartItem[]

  @@index([createdAt])
}

model CartItem {
  id        String   @id @default(cuid())
  userId    String
  productId String
  quantity  Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  createdAt DateTime @default(now())

  @@unique([userId, productId])
  @@index([userId])
}
```

#### Service con Cache-Aside

```typescript
// src/services/productService.ts
import { prisma } from '../lib/prisma';
import { redis } from '../lib/redis';

export class ProductService {
  // Cache-aside pattern
  async getProductById(id: string) {
    const cacheKey = `product:${id}`;
    
    // 1. Intentar leer de Redis
    const cached = await redis.get(cacheKey);
    if (cached) {
      console.log('✅ Cache hit');
      return JSON.parse(cached);
    }

    // 2. Cache miss - leer de BD
    console.log('❌ Cache miss - fetching from DB');
    const product = await prisma.product.findUnique({
      where: { id },
      select: { id: true, name: true, price: true, stock: true }
    });

    if (!product) throw new Error('Product not found');

    // 3. Guardar en Redis con TTL de 5 minutos
    await redis.setex(cacheKey, 300, JSON.stringify(product));
    
    return product;
  }

  // Invalidar cache cuando cambia el producto
  async invalidateProductCache(id: string) {
    await redis.del(`product:${id}`);
    await redis.del(`trending_products`);
  }

  // Agregar al carrito + invalidar cache
  async addToCart(userId: string, productId: string, quantity: number) {
    // Verificar stock
    const product = await prisma.product.findUniqueOrThrow({
      where: { id: productId },
    });

    if (product.stock < quantity) {
      throw new Error(`Insufficient stock. Available: ${product.stock}`);
    }

    // Usar transacción para atomicidad
    const result = await prisma.$transaction(async (tx) => {
      // Crear/actualizar carrito
      const cartItem = await tx.cartItem.upsert({
        where: {
          userId_productId: { userId, productId }
        },
        create: { userId, productId, quantity },
        update: { quantity: { increment: quantity } }
      });

      // Decrementar stock
      await tx.product.update({
        where: { id: productId },
        data: { stock: { decrement: quantity } }
      });

      return cartItem;
    });

    // Invalidar caches relevantes
    await this.invalidateProductCache(productId);
    await redis.del(`cart:${userId}`);
    
    return result;
  }

  // Productos trending con cache más largo
  async getTrendingProducts() {
    const cacheKey = 'trending_products';

    // Intentar cache
    const cached = await redis.get(cacheKey);
    if (cached) return JSON.parse(cached);

    // Query: top 10 productos más vendidos en últimos 7 días
    const trending = await prisma.product.findMany({
      where: {
        cartItems: {
          some: {
            createdAt: {
              gte: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)
            }
          }
        }
      },
      select: { id: true, name: true, price: true },
      orderBy: { cartItems: { _count: 'desc' } },
      take: 10
    });

    // Cache por 30 minutos
    await redis.setex(cacheKey, 1800, JSON.stringify(trending));

    return trending;
  }
}
```

#### Rutas Express

```typescript
// src/routes/products.ts
import express from 'express';
import { ProductService } from '../services/productService';

const router = express.Router();
const productService = new ProductService();

// GET un producto (cacheable)
router.get('/:id', async (req, res, next) => {
  try {
    const product = await productService.getProductById(req.params.id);
    res.json(product);
  } catch (err) {
    if (err.message === 'Product not found') {
      return res.status(404).json({ error: 'Not found' });
    }
    next(err);
  }
});

// GET top 10 productos populares
router.get('/trending', async (req, res, next) => {
  try {
    const trending = await productService.getTrendingProducts();
    res.json(trending);
  } catch (err) {
    next(err);
  }
});

// POST carrito
router.post('/cart', async (req, res, next) => {
  try {
    const { userId, productId, quantity } = req.body;
    
    // Validación
    if (!userId || !productId || quantity < 1) {
      return res.status(400).json({ error: 'Invalid input' });
    }

    const cartItem = await productService.addToCart(userId, productId, quantity);
    res.status(201).json(cartItem);
  } catch (err) {
    if (err.message.includes('Insufficient stock')) {
      return res.status(400).json({ error: err.message });
    }
    next(err);
  }
});

export default router;
```

---

### Paso 4: Revisión de Seguridad

**Comando:**
```
/security-scan
```

**El agente revisa:**
- ✅ Validación de entrada (userId, productId)
- ✅ N+1 queries evitadas (usando `select`)
- ✅ Race conditions en stock (transacción Prisma)
- ✅ Cache stampede prevention (fallbacks)
- ✅ No hay secretos en logs
- ⚠️ Rate limiting faltante en /cart POST
- ⚠️ Falta autenticación de usuario

---

### Riesgos y Cómo Evitarlos

| Riesgo | Problema | Solución ECC |
|--------|----------|-------------|
| **Cache Stampede** | 1000 requests simultáneos al expirar cache | Usar locks o `setex` preventivo |
| **Race condition en stock** | Dos usuarios compran último item simultáneamente | Transacción + `select for update` |
| **N+1 queries** | 1 GET trending = 100 queries | Usar `select` explícito, no `include` |
| **Memory leak Redis** | Keys sin TTL crecen infinitamente | Siempre usar `setex`, nunca `set` |
| **Stale cache** | Usuario ve producto agotado pero cache dice disponible | Invalidar en UPDATE/DELETE |

---

### Flujo Completo con ECC

```
1. /ecc:plan "e-commerce con Prisma + Redis"
   ↓ Agente crea plan de 4 fases
   
2. tdd-workflow "carrito con invalidación de cache"
   ↓ Escribe tests antes de implementar
   
3. Implementar código siguiendo plan + tests
   ↓ Verde ✅
   
4. /code-review
   ↓ Agente revisa desde contexto fresco
   
5. /security-scan
   ↓ Auditoría de seguridad automática
   
6. Resultado: API lista para producción
```

---

## 📊 Resumen Ejecutivo

### ¿Cuándo usar ECC?

| Escenario | ¿Usar ECC? |
|-----------|----------|
| Crear feature nueva | ✅ Sí - `/ecc:plan` |
| Escribir tests | ✅ Sí - `tdd-workflow` |
| Revisar código | ✅ Sí - `/code-review` |
| Auditar seguridad | ✅ Sí - `/security-scan` |
| Fixear errores de build | ✅ Sí - `/build-fix` |
| Limpiar código muerto | ✅ Sí - `/refactor-clean` |
| Usar Prisma | ✅ Sí - `prisma-patterns` |
| Usar Redis | ✅ Sí - `redis-patterns` |
| Combinar ambas | ✅ Sí - `backend-patterns` |

### Diferencia Sin ECC vs Con ECC

| Aspecto | Sin ECC | Con ECC |
|--------|--------|----------|
| **Planificación** | Ad-hoc | Sistemática, por fases |
| **Tests** | Opcional | Obligatorio (80%+ coverage) |
| **Seguridad** | A lo mejor | Checklist automático |
| **Revisión** | Manual | Automática desde contexto fresco |
| **Reutilización** | No | Memoria de patrones previos |
| **Consistencia** | Varía | Reglas del proyecto |
| **Tiempo de development** | ❓ | ⚡ Más rápido |

---

## 📚 Recursos y Enlaces

### Oficial
- **GitHub:** https://github.com/affaan-m/ECC
- **Website:** https://ecc.tools
- **Discord:** https://discord.gg/36yGMHGFbR
- **npm:** https://www.npmjs.com/package/ecc-universal

### Documentación
- **README completo:** https://github.com/affaan-m/ECC#readme
- **Guía de instalación:** https://github.com/affaan-m/ECC#install-ecc
- **Troubleshooting:** https://github.com/affaan-m/ECC/blob/main/TROUBLESHOOTING.md

### Skills Relevantes
- `prisma-patterns` - Patrones de Prisma ORM
- `redis-patterns` - Patrones de Redis
- `backend-patterns` - Patrones de backend en general
- `tdd-workflow` - Test-Driven Development
- `security-review` - Auditoría de seguridad
- `api-design` - Diseño de APIs RESTful
- `database-migrations` - Migraciones con Prisma

### Agentes
- `planner` - Planificación de features
- `code-reviewer` - Revisión de código
- `security-reviewer` - Auditoría de seguridad
- `tdd-guide` - Guía de TDD
- `build-error-resolver` - Resolución de errores de build

---

## 🎓 Comandos Útiles Rápidos

```bash
# Instalación
npx ecc-universal@2.2.1 setup

# En OpenCode - Planificación
/ecc:plan "descripción de la feature"

# En OpenCode - TDD
tdd-workflow

# En OpenCode - Revisión
/code-review

# En OpenCode - Seguridad
/security-scan

# En OpenCode - Fixear build
/build-fix

# En OpenCode - Limpiar código
/refactor-clean

# Consultar skills disponibles
/consult "tu pregunta aquí"

# Verificar instalación
npx ecc-universal doctor
```

---

## 📝 Notas Importantes

### Prisma + Redis
- **Siempre usar transacciones** para operaciones que afecten stock
- **Cache-aside es el patrón por defecto** para lecturas frecuentes
- **Invalidar cache en UPDATE/DELETE** para evitar stale data
- **No cachear datos sensibles** (contraseñas, tokens)
- **Siempre poner TTL** en Redis keys

### Errores Comunes
- ❌ Usar `updateMany` esperando records, cuando devuelve `{ count: n }`
- ❌ Dejar keys Redis sin TTL → memory leak
- ❌ Cache sin invalidación → datos obsoletos
- ❌ N+1 queries → performance degradado
- ❌ Race conditions en stock → overselling

### Best Practices
- ✅ Usar `prisma-patterns` y `redis-patterns` skills
- ✅ Tests antes de código (TDD)
- ✅ Revisar con fresh context (`/code-review`)
- ✅ Auditar seguridad (`/security-scan`)
- ✅ Documentar decisiones

---

## 📄 Licencia

Esta guía es de **código abierto** bajo licencia MIT, igual que ECC.

---

**Última actualización:** Septiembre 2026
**Versión de ECC:** 2.2.1
**Autor:** Compilado por dante2182