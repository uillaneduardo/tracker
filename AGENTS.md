# Tracker — Instruções para Agentes de IA

## Objetivo

O Tracker é uma plataforma self-hosted e open source para coleta, sincronização, armazenamento e análise de localização, movimento e telemetria de dispositivos explicitamente autorizados.

## Fonte de verdade

Antes de implementar qualquer mudança, leia nesta ordem:

1. `README.md` — visão geral.
2. `docs/PROJECT.md` — definição do produto e escopo.
3. `docs/REQUIREMENTS.md` — requisitos funcionais e não funcionais.
4. `docs/ARCHITECTURE.md` — arquitetura e limites dos componentes.
5. `docs/DATA_MODEL.md` — modelo conceitual dos dados.
6. `docs/ROADMAP.md` — prioridades e fases.
7. `docs/SECURITY.md` e `docs/PRIVACY.md` — requisitos obrigatórios.
8. `docs/DEVELOPMENT.md` — convenções de implementação.

Se houver conflito, não invente uma decisão silenciosamente. Preserve requisitos de segurança/privacidade e registre a decisão em `docs/adr/` quando ela alterar a arquitetura ou o contrato do produto.

## Regras de implementação

- Não implementar funcionalidades fora do escopo sem registrar a necessidade.
- Não remover requisitos documentados para simplificar uma implementação sem decisão explícita.
- Manter separação entre telemetria bruta, dados processados e atividades.
- Toda operação sensível deve ser autorizada no backend; o frontend não é uma fronteira de segurança.
- Dispositivos possuem identidade/credencial própria.
- Sincronização deve ser idempotente e tolerante a rede intermitente.
- Não criar rastreamento oculto, furtivo ou sem consentimento.
- Lost Mode somente para dispositivos próprios ou explicitamente autorizados.
- Evitar microserviços e infraestrutura distribuída prematuramente.
- Preferir componentes open source e implantação Docker/Compose no MVP.
- Mudanças de schema devem ser versionadas por migrations.
- APIs devem possuir contratos claros e documentação atualizada.
- Testes devem cobrir especialmente autorização, isolamento entre usuários, sincronização e processamento geográfico.

## Fluxo recomendado do agente

1. Inspecionar documentação e estado atual do código.
2. Identificar requisitos afetados.
3. Planejar a menor mudança coerente.
4. Implementar.
5. Executar testes/lint/build disponíveis.
6. Atualizar documentação quando o comportamento ou arquitetura mudar.
7. Resumir arquivos alterados, decisões e validações.

## Princípio importante

O Tracker deve preservar os dados brutos sempre que possível. Algoritmos de classificação, agrupamento e processamento podem mudar; o histórico bruto não deve ser destruído apenas para acomodar uma versão nova do algoritmo.
