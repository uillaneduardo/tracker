# API — Diretrizes e Contrato Inicial

Este documento define a direção do contrato HTTP. Endpoints concretos podem mudar antes do MVP, mas mudanças devem ser documentadas.

## Convenções

- API versionada, inicialmente `/api/v1`.
- JSON para recursos e metadados.
- UTC/ISO 8601 para timestamps.
- IDs opacos/UUID ou equivalente.
- Erros em formato estruturado.
- Paginação para consultas potencialmente grandes.
- Filtros temporais obrigatórios em consultas de histórico de grande volume.

## Recursos previstos

```text
POST   /api/v1/auth/...
GET    /api/v1/me
GET    /api/v1/devices
POST   /api/v1/devices
GET    /api/v1/devices/{id}
POST   /api/v1/devices/{id}/credentials/rotate
POST   /api/v1/telemetry/batches
GET    /api/v1/tracks
GET    /api/v1/tracks/{id}
GET    /api/v1/activities
GET    /api/v1/activities/{id}
POST   /api/v1/activities
GET    /api/v1/media
POST   /api/v1/media
GET    /api/v1/exports/...
GET    /api/v1/audit/...
```

## Ingestão

A ingestão deve aceitar lote de pontos e um identificador idempotente por ponto/evento ou lote.

Exemplo conceitual:

```json
{
  "device_id": "device-123",
  "batch_id": "batch-456",
  "points": [
    {
      "id": "point-1",
      "observed_at": "2026-09-10T03:00:00Z",
      "latitude": -8.05,
      "longitude": -34.90,
      "accuracy_m": 8.2,
      "altitude_m": 12.4,
      "speed_mps": 5.8,
      "bearing_deg": 142,
      "battery_percent": 74
    }
  ]
}
```

## Idempotência

Reenvio de `point-1` não deve criar um segundo ponto lógico. A API deve responder de maneira determinística e permitir ao cliente saber quais elementos foram aceitos, duplicados ou rejeitados.

## Autenticação de dispositivo

Dispositivo deve apresentar sua credencial própria. O backend deriva o `device_id` autorizado da identidade autenticada e não deve aceitar arbitrariamente um `device_id` pertencente a outro usuário.

## Consultas geográficas

Consultas devem permitir filtros como:

- intervalo de tempo;
- device;
- activity;
- bounding box/área;
- proximidade;
- tipo de atividade.

## Evolução do contrato

Mudanças incompatíveis exigem nova versão ou estratégia de compatibilidade documentada. Clientes móveis podem permanecer offline e atualizar posteriormente, portanto o servidor deve tolerar versões suportadas de cliente.
