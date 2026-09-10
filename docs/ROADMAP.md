# Roadmap

> Roadmap orientativo. Prioridade pode mudar conforme validação técnica e uso real.

## Fase 0 — Fundação

- [ ] estrutura do monorepo
- [ ] documentação e ADRs
- [ ] ambiente Docker Compose
- [ ] PostgreSQL + PostGIS
- [ ] API base
- [ ] migrations
- [ ] configuração/segredos
- [ ] logs e health checks
- [ ] CI básica

## Fase 1 — Backend e identidade

- [ ] usuários
- [ ] autenticação
- [ ] RBAC/ACL
- [ ] dispositivos
- [ ] credenciais por dispositivo
- [ ] auditoria
- [ ] ingestão de telemetria
- [ ] idempotência
- [ ] testes de isolamento entre usuários

## Fase 2 — Android MVP

- [ ] registro/autorização do dispositivo
- [ ] permissões de localização
- [ ] coleta em background
- [ ] GPS, precisão, altitude, velocidade e direção
- [ ] bateria e estado do dispositivo
- [ ] armazenamento local
- [ ] fila offline
- [ ] sincronização por lote
- [ ] configuração de frequência
- [ ] configuração de rede

## Fase 3 — Web MVP

- [ ] login
- [ ] dashboard
- [ ] dispositivos
- [ ] mapa
- [ ] histórico por período
- [ ] tracks
- [ ] atividades básicas
- [ ] configurações de privacidade
- [ ] exportação inicial

## Fase 4 — Contexto

- [ ] classificação automática/híbrida
- [ ] lugares
- [ ] geofences
- [ ] permanência
- [ ] timeline
- [ ] fotos
- [ ] vídeos
- [ ] notas/eventos
- [ ] zonas privadas

## Fase 5 — Exercícios

- [ ] caminhada
- [ ] corrida
- [ ] ciclismo
- [ ] métricas avançadas
- [ ] sensores externos quando suportados
- [ ] histórico e comparação de atividades

## Fase 6 — Outros dispositivos

- [ ] desktop agent
- [ ] notebook
- [ ] API para trackers externos
- [ ] capacidades/sensores extensíveis

## Fase 7 — Recuperação

- [ ] Lost Mode
- [ ] aumento de frequência
- [ ] prioridade de sincronização
- [ ] última posição/status
- [ ] auditoria específica

## Fase 8 — Cloud

- [ ] multi-tenant completo
- [ ] alta disponibilidade
- [ ] backups automatizados
- [ ] object storage escalável
- [ ] observabilidade
- [ ] atualizações gerenciadas
- [ ] documentação operacional
- [ ] suporte e billing se o produto comercial for criado

## Regra de priorização

Priorizar nesta ordem:

1. segurança e privacidade;
2. integridade dos dados;
3. confiabilidade da coleta/sincronização;
4. experiência básica de consulta;
5. processamento/contexto;
6. recursos avançados;
7. escala comercial.
