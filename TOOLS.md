# TOOLS

Documentación de las APIs expuestas por el backend (Django REST Framework) del asistente Soul / MiAgente-Aaron.

Cada skill/funcionalidad tiene su propio endpoint documentado a continuación.

## get_nearby_locations
Endpoint para obtener lugares cercanos usando coordenadas geográficas.

**Método**: GET  
**URL**: `/api/locations/nearby/`

**Parámetros recibidos (query params)**:
- `lat` (float, requerido): Latitud del punto de referencia (ej: 32.5149 para Tijuana).
- `lon` (float, requerido): Longitud del punto de referencia (ej: -117.0382).
- `id` (int, opcional): ID de un usuario o sesión para personalizar resultados (si aplica).

**Qué hace**:
- Recibe latitud y longitud del usuario.
- Consulta una base de datos o servicio externo (ej: Google Places, OpenStreetMap, o modelo propio).
- Devuelve una lista JSON de lugares cercanos ordenados por distancia.
- Posible paginación y filtros adicionales (radio máximo, tipo de lugar, etc.).

**Ejemplo de request**:


**Ejemplo de response (JSON)**:
```json
{
  "status": "success",
  "locations": [
    {
      "id": 123,
      "name": "Tacos El Franc",
      "address": "Av. Revolución 1234",
      "distance_km": 0.8,
      "lat": 32.5182,
      "lon": -117.0351
    },
    ...
  ],
  "count": 15
}