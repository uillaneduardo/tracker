# Requisitos

## 1. Requisitos funcionais

### RF-001 — Usuários
O sistema deve permitir criar, autenticar, desativar e administrar usuários.

### RF-002 — Dispositivos
Cada dispositivo deve possuir identidade própria, proprietário, estado e credencial independente.

### RF-003 — Consentimento e autorização
A coleta deve estar vinculada a autorização explícita e revogável do proprietário/usuário.

### RF-004 — Telemetria
A API deve aceitar pontos com timestamp, latitude, longitude e campos opcionais como altitude, velocidade, direção, precisão, bateria, movimento e sensores.

### RF-005 — Armazenamento bruto
O sistema deve preservar os pontos brutos recebidos, sujeitos às políticas de retenção configuradas.

### RF-006 — Offline-first
O cliente deve armazenar localmente os dados ainda não sincronizados.

### RF-007 — Sincronização
A sincronização deve suportar lotes, retry, interrupção e idempotência.

### RF-008 — Tracks
O backend deve permitir derivar trajetos de sequências de telemetria.

### RF-009 — Atividades
O sistema deve representar atividades manuais, automáticas ou híbridas.

### RF-010 — Classificação
A arquitetura deve permitir evoluir algoritmos de classificação sem destruir os dados brutos.

### RF-011 — Mapa e histórico
A interface web deve permitir consultar localização, tracks e atividades por período.

### RF-012 — Exercícios
Deve existir estrutura para métricas de caminhada, corrida, ciclismo e outros exercícios.

### RF-013 — Mídia
Fotos, vídeos, notas e eventos devem poder ser associados a atividades e/ou posições.

### RF-014 — Privacidade
O usuário deve controlar precisão, frequência, retenção, sincronização, rastreamento em segundo plano e compartilhamento quando aplicável.

### RF-015 — Zonas privadas
A arquitetura deve permitir regras específicas para áreas privadas.

### RF-016 — Exportação
Usuários autorizados devem poder exportar seus dados em formatos documentados.

### RF-017 — Lost Mode
Dispositivos próprios/autorizados podem entrar em modo de recuperação com política de coleta diferenciada.

### RF-018 — RBAC
O sistema deve suportar papéis e permissões, combinados com propriedade dos recursos.

### RF-019 — Auditoria
Ações de autenticação, acesso sensível, permissões, exportação, dispositivos e recuperação devem gerar audit log.

### RF-020 — Multiusuário
Os dados de um usuário não podem ser acessados por outro usuário sem autorização explícita.

## 2. Requisitos não funcionais

### RNF-001 — Segurança
Toda comunicação de rede deve utilizar TLS em ambientes de produção. Segredos não devem ser armazenados no código.

### RNF-002 — Autorização
Toda autorização deve ser validada no backend em cada operação protegida.

### RNF-003 — Desempenho
O armazenamento deve suportar grandes volumes de pontos sem exigir que cada consulta carregue todo o histórico.

### RNF-004 — Escalabilidade
A arquitetura deve permitir separar API, processamento e armazenamento posteriormente.

### RNF-005 — Confiabilidade
Perdas de conectividade não devem causar perda silenciosa de telemetria já armazenada localmente.

### RNF-006 — Idempotência
Reenvio do mesmo lote/ponto não deve criar duplicação lógica.

### RNF-007 — Manutenibilidade
Componentes devem possuir responsabilidades claras, testes e documentação suficiente para agentes e desenvolvedores.

### RNF-008 — Portabilidade
O MVP deve ser executável em infraestrutura comum via Docker Compose.

### RNF-009 — Observabilidade
Serviços devem fornecer logs estruturados e indicadores básicos de saúde.

### RNF-010 — Privacidade
Coleta e retenção devem ser minimizadas conforme a finalidade configurada.

## 3. Critérios gerais de aceite

Uma funcionalidade é considerada pronta quando:

1. atende aos requisitos relevantes;
2. possui testes apropriados;
3. não viola isolamento/autorização;
4. possui tratamento de falhas esperado;
5. documentação/contrato afetado foi atualizado;
6. pode ser operada no ambiente suportado;
7. mudanças de dados possuem migration/versionamento quando necessário.
