# Desenvolvimento

## Estrutura planejada

```text
tracker/
├── api/          # backend HTTP, autenticação, ingestão e consultas
├── web/          # React/TypeScript web/PWA
├── worker/       # processamento assíncrono
├── mobile/       # Android/Kotlin
├── agent/        # desktop/notebook
├── docker/       # compose, imagens e operação local
├── docs/         # documentação técnica e produto
└── README.md
```

## Direção tecnológica

| Área | Direção inicial |
|---|---|
| Backend | Go |
| Banco | PostgreSQL + PostGIS |
| Web | React + TypeScript |
| Android | Kotlin |
| Desktop | Go ou Rust |
| Infra MVP | Docker Compose |
| Media | volume local e/ou S3-compatible |
| Cache/queue | Redis opcional |

Essas escolhas são direcionais, não dogmas. Uma mudança deve considerar custo operacional, desempenho, portabilidade e manutenção.

## Convenções

- Código simples antes de abstrações prematuras.
- Funções/módulos com responsabilidade única.
- Configuração por ambiente.
- Sem segredos versionados.
- Migrations versionadas.
- Testes próximos ao comportamento que protegem.
- Logs úteis sem dados sensíveis desnecessários.
- Timezones internas preferencialmente UTC.
- Distâncias/velocidades devem possuir unidades explícitas no contrato interno/externo.

## Branch/commit

Usar mensagens de commit claras e focadas. Evitar commits que misturem refatoração ampla com mudança funcional sem necessidade.

## Testes mínimos

Backend:
- autenticação;
- autorização/ownership;
- ingestão idempotente;
- validação geográfica;
- processamento de tracks;
- retenção/exportação.

Android:
- fila offline;
- retry;
- idempotência;
- permissões;
- coleta em background quando possível testar;
- comportamento sem rede.

Web:
- fluxos críticos;
- isolamento de dados apresentado;
- estados vazios/erro/offline;
- mapas e filtros.

## Alterações de banco

Nunca alterar schema de produção manualmente como parte normal do desenvolvimento. Criar migration reversível quando possível, testar upgrade a partir de uma instalação anterior e documentar mudanças de dados relevantes.

## Definition of Done

Uma tarefa está concluída quando código, testes, documentação, migrations e configuração necessária estão coerentes e a validação disponível foi executada.
