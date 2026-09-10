# Tracker

> Plataforma **self-hosted e open source** para coleta, sincronização, histórico e análise de localização, movimento e telemetria de dispositivos autorizados.

**Status:** planejamento / fundação arquitetural

## Visão

O Tracker transforma telemetria de dispositivos autorizados em histórico espacial, trajetos, atividades, eventos, métricas e contexto. O projeto é **self-hosted first**, **offline-first**, multiusuário e orientado à privacidade.

O núcleo deve preservar a telemetria bruta para que tracks, atividades e algoritmos possam ser recalculados no futuro sem perder a fonte original.

## Arquitetura resumida

```text
Android ───────┐
Desktop ───────┼──> API/Auth ──> PostgreSQL + PostGIS
External API ──┘       │                │
                        └──> Worker      ├── raw telemetry
                             │           ├── tracks
Web/PWA ───────────────> API             ├── activities
                                         └── spatial data
                                                  │
                                             Object Storage
                                             photos / videos
```

O MVP evita microserviços e Kubernetes prematuros. A implantação inicial deve ser simples, preferencialmente com Docker Compose.

## Stack direcionada

| Componente | Direção |
|---|---|
| Backend | Go |
| Banco | PostgreSQL + PostGIS |
| Web/PWA | React + TypeScript |
| Android | Kotlin |
| Desktop | Go ou Rust |
| Infraestrutura MVP | Docker Compose |
| Media | volume local / S3-compatible |
| Queue/cache | Redis opcional |

## Domínio principal

```text
User
 └── Device
      └── TelemetryPoint
           └── Track
                └── Activity
                     ├── Media
                     ├── Event
                     └── Place

User ──> AuditLog
```

Fontes previstas: Android, desktop/notebook, trackers de hardware e APIs externas.

## Capacidades planejadas

- coleta de GPS, velocidade, altitude, direção, precisão, bateria, movimento e sensores;
- armazenamento local e sincronização offline-first;
- histórico e mapas;
- tracks e métricas;
- classificação de atividades;
- caminhada, corrida, ciclismo e outras atividades;
- fotos, vídeos, notas e eventos contextualizados;
- lugares, geofences e zonas privadas;
- retenção, precisão e frequência configuráveis;
- multiusuário, RBAC e ownership;
- auditoria de operações sensíveis;
- exportação dos dados autorizados;
- Lost Mode para dispositivos próprios/autorizados;
- futura integração com desktop e trackers externos;
- caminho para uma edição Cloud com alta disponibilidade.

## Documentação do projeto

A documentação detalhada foi separada do README para que humanos e agentes de IA possam navegar pelo projeto sem transformar o README em uma especificação monolítica.

| Documento | Finalidade |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Regras para agentes de IA, fonte de verdade e fluxo de trabalho |
| [`docs/PROJECT.md`](docs/PROJECT.md) | Visão, objetivos, escopo e casos de uso |
| [`docs/REQUIREMENTS.md`](docs/REQUIREMENTS.md) | Requisitos funcionais, não funcionais e aceite |
| [`docs/FEATURES.md`](docs/FEATURES.md) | Inventário de funcionalidades e estado planejado |
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | Componentes, fluxos e decisões arquiteturais |
| [`docs/DATA_MODEL.md`](docs/DATA_MODEL.md) | Entidades, relações e regras de integridade |
| [`docs/API.md`](docs/API.md) | Diretrizes do contrato da API |
| [`docs/SECURITY.md`](docs/SECURITY.md) | Modelo inicial de segurança e ameaças |
| [`docs/PRIVACY.md`](docs/PRIVACY.md) | Privacidade, retenção, precisão e consentimento |
| [`docs/DEVELOPMENT.md`](docs/DEVELOPMENT.md) | Stack, convenções, testes e Definition of Done |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | Fases e prioridades do projeto |
| [`docs/adr/`](docs/adr/) | Decisões arquiteturais registradas |

## Estrutura do repositório

```text
tracker/
├── AGENTS.md
├── README.md
├── api/
├── web/
├── worker/
├── mobile/
├── agent/
├── docker/
├── docs/
│   ├── PROJECT.md
│   ├── REQUIREMENTS.md
│   ├── FEATURES.md
│   ├── ARCHITECTURE.md
│   ├── DATA_MODEL.md
│   ├── API.md
│   ├── SECURITY.md
│   ├── PRIVACY.md
│   ├── DEVELOPMENT.md
│   ├── ROADMAP.md
│   └── adr/
└── ...
```

Diretórios de implementação serão preenchidos conforme cada fase do roadmap começar. A estrutura documental não deve depender da existência imediata de todos os componentes.

## Princípios não negociáveis

1. Rastreamento somente com autorização apropriada.
2. Nenhum mecanismo de rastreamento oculto/furtivo.
3. Autorização sempre validada no backend.
4. Isolamento entre usuários desde o início.
5. Identidade/credencial própria por dispositivo.
6. Telemetria bruta preservada enquanto permitida pela política de retenção.
7. Sincronização idempotente e tolerante a falhas.
8. Privacidade tratada como requisito arquitetural.
9. Evitar complexidade operacional sem necessidade real.

## Primeira entrega

A primeira implementação deve priorizar **fundação + backend + ingestão confiável + Android MVP + web básica**, nessa ordem aproximada. Ver [`docs/ROADMAP.md`](docs/ROADMAP.md).

## Licença

A licença ainda será definida antes da primeira release pública.
