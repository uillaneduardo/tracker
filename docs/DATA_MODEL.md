# Modelo de Dados

## 1. Entidades principais

```text
User
 ├── Device
 │    └── TelemetryPoint
 │          └── Track
 │                └── Activity
 │                     ├── Media
 │                     ├── Event
 │                     └── Place
 └── AuditLog
```

## 2. User

Representa uma conta. Deve possuir estado, papel/permissões e políticas de privacidade aplicáveis.

Campos conceituais: `id`, `email/username`, `status`, `role`, timestamps.

## 3. Device

Representa um cliente autorizado.

Campos conceituais:
- `id`;
- `user_id`;
- `name`;
- `type`;
- `status`;
- `capabilities`;
- `last_seen_at`;
- `last_location_at`;
- `battery` quando disponível;
- timestamps.

## 4. Device Credential

Credencial independente por dispositivo. Deve permitir revogação e rotação sem alterar a conta do usuário.

Nunca armazenar segredo recuperável em texto puro quando um hash/credencial equivalente puder ser utilizado.

## 5. TelemetryPoint

Dado bruto e imutável no sentido lógico após ingestão, salvo correções administrativas explicitamente auditadas.

Campos mínimos:
- `id` ou identificador idempotente;
- `device_id`;
- `observed_at`;
- `position` PostGIS;
- `accuracy` opcional.

Campos opcionais:
- `altitude`;
- `speed`;
- `bearing`;
- `battery`;
- `motion/activity hint`;
- sensores;
- dados de rede;
- metadados de origem.

## 6. Track

Sequência processada de pontos representada como geometria, com início/fim e métricas derivadas.

Campos conceituais: `id`, `device_id`, `start_at`, `end_at`, `geometry`, `distance`, `duration`, `elevation_gain`, `elevation_loss`, `processing_version`.

## 7. Activity

Contexto semântico de um track ou conjunto de tracks.

Tipos iniciais: `walk`, `run`, `cycling`, `commute`, `travel`, `outing`, `transport`, `custom`.

Classificação deve registrar método/versão para permitir reprocessamento e explicabilidade futura.

## 8. Media

Referência para objeto armazenado externamente ao banco quando apropriado.

Metadados: `id`, `owner`, `storage_key`, `mime_type`, `captured_at`, `position`, `activity_id`, `device_id`.

## 9. Place / Geofence

Estrutura futura para locais conhecidos, zonas privadas e regras espaciais.

## 10. Event

Ocorrência contextual, como início/fim de atividade, entrada/saída de zona, alteração de dispositivo ou evento criado pelo usuário.

## 11. AuditLog

Registro append-oriented de ações sensíveis.

Campos conceituais: `id`, `actor`, `action`, `resource_type`, `resource_id`, `timestamp`, `source`, `metadata`.

## 12. Regras de integridade

- Todo recurso de usuário deve possuir vínculo inequívoco de ownership ou ACL.
- Pontos devem possuir timestamp confiável e coordenadas válidas quando posição estiver presente.
- IDs usados para ingestão devem permitir idempotência.
- Dados processados devem identificar a versão do processamento.
- Exclusão/retention de dados deve respeitar dependências e auditoria.
