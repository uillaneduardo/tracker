# ADR 0001 — Arquitetura inicial

- Status: Accepted
- Data: 2026-09-10

## Contexto

O Tracker precisa suportar localização, telemetria, mapas, histórico, processamento e múltiplos usuários, mas deve continuar simples de operar em infraestrutura self-hosted.

## Decisão

O MVP utilizará uma arquitetura modular com API, worker, web, cliente Android e PostgreSQL/PostGIS. Docker Compose será o mecanismo inicial de implantação. Redis/filas e separação adicional de serviços serão opcionais conforme necessidade.

Backend preferencial: Go.
Web: React + TypeScript.
Android: Kotlin.

## Motivos

- baixo overhead operacional;
- boa adequação a APIs concorrentes;
- PostGIS é adequado ao domínio espacial;
- clientes podem evoluir independentemente;
- evita Kubernetes/microserviços prematuros;
- preserva caminho para escala futura.

## Consequências

A primeira versão terá alguns componentes bem definidos, mas não uma arquitetura distribuída completa. O worker poderá ser escalado posteriormente. A API deve manter contratos estáveis para clientes móveis que podem ficar offline.
