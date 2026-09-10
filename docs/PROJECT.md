# Definição do Projeto

## 1. Visão

O Tracker é uma plataforma de histórico espacial e telemetria pessoal, multiusuário, self-hosted e open source. O sistema coleta dados de dispositivos autorizados, preserva a telemetria original e transforma esses dados em trajetos, atividades, eventos, estatísticas e contexto geográfico.

A visão não é criar apenas um rastreador GPS. O produto deve funcionar como uma camada pessoal de memória espacial e telemetria.

## 2. Objetivos

- Registrar localização e telemetria de dispositivos autorizados.
- Funcionar offline-first em dispositivos móveis.
- Sincronizar dados de forma confiável e idempotente.
- Permitir consultar histórico em mapa e linha do tempo.
- Transformar pontos em tracks e atividades.
- Associar fotos, vídeos, notas e eventos ao contexto geográfico/temporal.
- Oferecer métricas para deslocamentos e exercícios.
- Suportar múltiplos usuários com isolamento de dados.
- Permitir implantação self-hosted simples.
- Criar base técnica para uma futura oferta hospedada com alta disponibilidade.

## 3. Fora do objetivo

No MVP não são objetivos:

- microserviços complexos;
- Kubernetes;
- hardware proprietário obrigatório;
- rastreamento de terceiros sem autorização;
- mecanismos furtivos de vigilância;
- depender de serviços proprietários para o funcionamento básico;
- construir uma rede social de localização.

## 4. Personas

### Usuário pessoal
Quer consultar seu próprio histórico, deslocamentos, atividades e dispositivos.

### Administrador self-hosted
Opera a instalação, usuários, dispositivos, armazenamento, segurança e manutenção.

### Dispositivo autorizado
Android, desktop, notebook, tracker ou integração externa que envia telemetria.

### Futuro operador Cloud
Administra uma instalação hospedada multi-tenant, sem alterar o núcleo funcional do produto.

## 5. Casos de uso principais

1. Cadastrar um dispositivo.
2. Autorizar o dispositivo para coleta.
3. Coletar pontos sem conexão.
4. Sincronizar quando a conexão retornar.
5. Visualizar posição atual e última posição.
6. Consultar histórico por período.
7. Visualizar um trajeto no mapa.
8. Criar ou classificar uma atividade.
9. Consultar métricas de atividade.
10. Associar mídia a atividade/local/horário.
11. Configurar frequência e precisão.
12. Configurar retenção e privacidade.
13. Exportar dados autorizados.
14. Ativar Lost Mode em dispositivo próprio/autorizado.
15. Auditar ações sensíveis.

## 6. Princípios de produto

- Privacy by default.
- Self-hosted first.
- Offline first.
- Raw data preservation.
- Backend-enforced authorization.
- Explicit device identity.
- Extensibilidade sem complexidade prematura.
- Open standards sempre que possível.
- Observabilidade e manutenção como requisitos, não extras.
