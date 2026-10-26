# Notas de configuración
## CLAUDE.md
Incluí: descripción del proyecto, comandos principales (npm run dev, npm test, npm run lint), convenciones (ES modules, naming en camelCase), y arquitectura (server.js como entry point, rutas en routes/, datos en db/store.js).
Dejé afuera: detalles de implementación obvios del código, tareas específicas, y cualquier cosa sensible. Quería que fuera conciso y útil desde el primer momento.
## Permisos
Agregué allow para npm test (comando seguro que uso seguido), y deny para .env (contiene secrets) y git push --force (puede destruir historial). Sin la regla deny, Claude podría accidentalmente exponer mis variables de entorno o hacer un force push que borre commits.