# Arquitetura

## 1. Direção arquitetural

O MVP deve utilizar poucos componentes com fronteiras claras. A arquitetura deve permitir evolução sem introduzir microserviços antes de existir necessidade operacional.

```text
Android ───────┐
Desktop Agent ─┼──> API/Auth ──> PostgreSQL + PostGIS
External API ──┘       │                │
                        │                ├── raw telemetry
                        │                ├── tracks
                        │                ├── activities
                        │                └── spatial data
                        │
                        ├── Processing Worker
                        │
                        └── Object Storage
                                 │
                              media

Web/PWA ───────────────> API
```

## 2. Componentes

### API
Responsável por autenticação, autorização, usuários, dispositivos, ingestão de telemetria, consultas, atividades, exportações e configurações.

Direção inicial: Go.

### PostgreSQL + PostGIS
Fonte principal de dados estruturados e geográficos.

Responsabilidades:
- usuários e ACL;
- dispositivos;
- credenciais/estado dos dispositivos;
- telemetria;
- tracks;
- atividades;
- eventos/lugares/geofences;
- auditoria.

### Processing Worker
Processa telemetria assíncrona para derivar tracks, métricas, atividades, lugares e outras informações.

Pode começar como processo separado simples e utilizar fila somente quando necessário.

### Object Storage
Armazena fotos, vídeos e outros blobs. Deve suportar armazenamento local e, futuramente, S3-compatible.

### Web/PWA
Interface de administração e consulta. Direção: React + TypeScript.

### Android
Cliente nativo Kotlin. Responsável por coleta, armazenamento local, permissões do sistema, sincronização e UX de atividades.

### Desktop Agent
Agente futuro, preferencialmente Go ou Rust, para notebooks/desktops.

## 3. Fluxo de ingestão

```text
Sensor
 -> client collector
 -> local durable queue
 -> sync batch
 -> authenticated API
 -> validation
 -> raw telemetry
 -> processing
 -> track/activity
```

A ingestão deve validar identidade do dispositivo, schema, timestamp, coordenadas e limites operacionais antes de aceitar os dados.

## 4. Separação de dados

O sistema deve manter três níveis conceituais:

1. **Raw** — observação original recebida do dispositivo.
2. **Processed** — tracks, métricas e derivados calculados.
3. **Context** — atividades, lugares, eventos, mídia e classificações.

Um novo algoritmo deve poder recalcular derivados usando os dados raw, sem exigir nova coleta do dispositivo.

## 5. Processamento

O processamento deve ser desacoplado da ingestão sempre que possível. Uma falha no processamento não deve apagar o dado bruto já aceito.

Operações candidatas:
- ordenação temporal;
- limpeza/validação;
- agrupamento em tracks;
- cálculo de distância;
- velocidade e duração;
- elevação;
- detecção de permanência;
- classificação de atividade;
- associação com lugares/geofences.

## 6. Escalabilidade futura

A implantação inicial pode ser:

```text
Docker Compose
├── tracker-api
├── tracker-worker
├── tracker-web
├── postgres-postgis
└── object-storage (optional)
```

Redis/queue pode ser adicionado quando volume ou processamento justificar. Em ambiente Cloud, API e workers podem ser escalados horizontalmente, enquanto PostgreSQL/object storage passam a utilizar infraestrutura de alta disponibilidade adequada.

## 7. Princípios de fronteira

- API é a autoridade de autorização.
- Banco não deve ser exposto diretamente a clientes.
- Dispositivos nunca recebem privilégios administrativos.
- Worker não deve contornar ACL para expor dados.
- Media storage deve utilizar referências autorizadas pela API.
- Web não deve conter lógica de segurança que não seja repetida/enforced no backend.
