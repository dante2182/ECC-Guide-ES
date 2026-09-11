# Preguntas y Respuestas - ECC Guide ES

Este documento reúne todas las preguntas que hiciste durante la conversación junto con sus respuestas detalladas.

## Pregunta 1: ¿Qué es ECC?

**Q:** Explicame que hace este repo, como se instala en OpenCode, como se usa, dame un caso de uso real en creación un backend o frontend.

**A:** ECC (Everything Claude Code) es un sistema operativo para agentes de IA que proporciona:
- 68 agentes especializados
- 291 habilidades para diferentes contextos
- Reglas y hooks para automatización
- Memoria para aprendizaje continuo

[Ver respuesta completa en README.md - Sección "¿Qué es ECC?"]

---

## Pregunta 2: Prisma + Redis

**Q:** Si yo uso Prisma como ORM en la base de datos y Redis para el caching, ¿este repo sirve? ¿Qué haría y cómo se lo pediría caso de uso real?

**A:** Sí, ECC sirve perfectamente. Tiene dos habilidades específicas:
- `prisma-patterns`: Para queries, transacciones, migrations
- `redis-patterns`: Para caching, locks, rate limiting

[Ver respuesta completa en README.md - Sección "Prisma + Redis: ¿Sirve ECC?" y "Caso Real: Sistema de E-commerce"]

---

## Casos de Uso Cubiertos

### Backend REST API
- Crear endpoints CRUD
- Autenticación con JWT
- Validación de entrada
- Tests con TDD
- Auditoría de seguridad

### E-commerce con Caching
- Productos con cache-aside
- Carrito con transacciones
- Productos trending
- Invalidación de cache automática
- Prevención de overselling

---

## Recursos Clave

- GitHub: https://github.com/affaan-m/ECC
- Website: https://ecc.tools
- npm: ecc-universal@2.2.1

---

## Próximos Pasos

1. Instalar ECC: `npx ecc-universal@2.2.1 setup`
2. Crear plan: `/ecc:plan "tu feature"`
3. Escribir tests: `tdd-workflow`
4. Revisar código: `/code-review`
5. Auditar seguridad: `/security-scan`
