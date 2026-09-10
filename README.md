# Tracker

> Plataforma **self-hosted e open source** para coleta, sincronização, histórico e análise de localização, movimento e telemetria de dispositivos autorizados.

[![Status](https://img.shields.io/badge/status-planning-blue)](https://github.com/uillaneduardo/tracker)
[![Self-hosted](https://img.shields.io/badge/deployment-self--hosted-success)](https://github.com/uillaneduardo/tracker)
[![Open Source](https://img.shields.io/badge/source-open%20source-brightgreen)](https://github.com/uillaneduardo/tracker)

---

## Visão geral

O **Tracker** é um projeto de plataforma pessoal de telemetria e histórico espacial. A proposta é permitir que usuários coletem, sob autorização explícita, informações de localização e movimento de seus próprios dispositivos e consultem esses dados posteriormente por uma interface web.

O projeto foi concebido para funcionar **self-hosted**, com os dados sob controle do operador da instalação, e também com possibilidade futura de oferecer uma versão hospedada com alta disponibilidade.

O Tracker não pretende ser apenas um rastreador GPS. A visão de longo prazo é construir uma plataforma capaz de transformar dados brutos de sensores e localização em **trajetos, atividades, eventos, estatísticas e histórico contextualizado**.

### O que o Tracker pretende responder

- Onde estava meu dispositivo em determinado momento?
- Qual trajeto foi realizado?
- Quanto tempo durou um deslocamento?
- Qual foi a distância percorrida?
- Qual foi a velocidade média ou máxima?
- Qual foi a altitude registrada?
- Que atividades aconteceram durante determinado período?
- Quais fotos, vídeos ou notas estão relacionadas a uma atividade?
- Quais dispositivos estavam associados a determinado histórico?
- Onde está um dispositivo perdido, quando o rastreamento de recuperação estiver autorizado e ativo?

---

## Princípios do projeto

### 🔐 Privacidade por padrão

Dados de localização são altamente sensíveis. O Tracker deve tratar privacidade como requisito arquitetural, e não como recurso opcional.

A coleta deve ocorrer somente com autorização do usuário e o sistema deve oferecer controles claros sobre:

- quais dados são coletados;
- frequência de coleta;
- frequência de sincronização;
- uso de rede móvel ou Wi-Fi;
- retenção do histórico;
- precisão da localização armazenada ou exibida;
- dispositivos autorizados;
- compartilhamento de informações;
- acesso administrativo e auditoria.

### 🧩 Self-hosted primeiro

A instalação deve ser possível em infraestrutura própria, preferencialmente utilizando containers e componentes open source.

A arquitetura também deve permitir uma futura oferta hospedada sem criar uma versão completamente diferente do produto.

### 📡 Offline-first

O dispositivo não deve depender de conectividade contínua para registrar sua atividade.

Os dados devem poder ser armazenados localmente e sincronizados posteriormente quando a conectividade estiver disponível.

### 📈 Dados brutos preservados

O sistema deve separar dados brutos de telemetria dos dados processados.

Isso permite melhorar os algoritmos de classificação e processamento no futuro sem perder o histórico original.

---

# Arquitetura conceitual

```text
                         ┌──────────────────────┐
                         │       Web / PWA       │
                         │ Dashboard · Mapas     │
                         │ Histórico · Gestão    │
                         └──────────┬───────────┘
                                    │
                                    ▼
┌────────────────┐          ┌──────────────────┐
│ Android App    │─────────▶│       API        │
└────────────────┘          │ Auth · Devices   │
                            │ Telemetry · ACL  │
┌────────────────┐          └────────┬─────────┘
│ Desktop Agent  │───────────────────┤
└────────────────┘                    │
                                      ▼
                              ┌───────────────┐
                              │ PostgreSQL +  │
                              │    PostGIS    │
                              └───────┬───────┘
                                      │
                         ┌────────────┴────────────┐
                         ▼                         ▼
                  Processing Engine          Object Storage
                  Tracks / Activities        Photos / Videos
```

A arquitetura inicial deve evitar complexidade prematura. O MVP pode ser implementado como um conjunto pequeno de serviços bem definidos, evoluindo posteriormente para componentes independentes quando houver necessidade real.

---

# Fontes de dados

O Tracker deve ser capaz de receber dados de diferentes tipos de dispositivos.

| Fonte | Exemplos de dados |
|---|---|
| 📱 Android | GPS, velocidade, altitude, direção, atividade, bateria, sensores |
| 🏷️ Tracker/Tag | posição, movimento, bateria e sensores disponíveis |
| 💻 Notebook | localização disponível, rede, movimento e telemetria do dispositivo |
| 🖥️ Desktop | localização configurada, rede e telemetria disponível |
| 🔌 API | dispositivos e sistemas externos capazes de enviar telemetria |

Nem todo dispositivo terá os mesmos sensores. O modelo de dados deve, portanto, ser extensível e permitir campos opcionais e capacidades específicas por dispositivo.

---

# Modelo de dados

Um dos princípios centrais do Tracker é separar **telemetria**, **trajeto** e **atividade**.

```text
User
 │
 ├── Device
 │     │
 │     └── TelemetryPoint
 │              │
 │              └── Track
 │                    │
 │                    └── Activity
 │                           ├── Media
 │                           ├── Events
 │                           └── Places
 │
 └── AuditLog
```

## User

Representa a conta e o proprietário dos dados.

## Device

Representa um dispositivo autorizado a enviar informações.

Um usuário pode possuir diversos dispositivos:

```text
Usuário
├── Smartphone
├── Notebook
├── Desktop
└── GPS Tracker
```

Cada dispositivo deve possuir identidade e credenciais próprias para comunicação com a API.

## TelemetryPoint

Representa uma observação bruta enviada pelo dispositivo.

Exemplo conceitual:

```json
{
  "device_id": "device-123",
  "timestamp": "2026-09-10T03:00:00Z",
  "latitude": -8.05,
  "longitude": -34.90,
  "altitude": 12.4,
  "speed": 5.8,
  "bearing": 142,
  "accuracy": 8.2,
  "battery": 74,
  "motion": "walking"
}
```

Os campos devem ser adaptáveis às capacidades de cada dispositivo.

## Track

Representa um trajeto processado a partir de uma sequência de pontos de telemetria.

Pode ser utilizado para representar uma linha geográfica, distância, duração, elevação e outras métricas.

## Activity

Representa uma interpretação contextual do trajeto.

Exemplos:

- caminhada;
- corrida;
- ciclismo;
- deslocamento para o trabalho;
- viagem;
- passeio;
- transporte;
- atividade personalizada.

A classificação pode ser manual, automática ou híbrida.

## Media

Fotos, vídeos e outros conteúdos podem ser associados a atividades e pontos geográficos.

Isso permite relacionar uma fotografia ao local e horário em que foi capturada.

## AuditLog

Registra operações relevantes realizadas na plataforma, especialmente ações relacionadas a acesso, permissões, visualização e exportação de dados.

---

# Localização e dados geográficos

O banco principal previsto para o projeto é **PostgreSQL com PostGIS**.

A utilização de dados geográficos nativos permite trabalhar com:

- pontos de localização;
- linhas de trajeto;
- distância;
- áreas e geofences;
- permanência em locais;
- histórico espacial;
- consultas geográficas;
- agrupamento de atividades por região.

Isso evita limitar o projeto a simples pares de latitude/longitude.

---

# Coleta e sincronização

O aplicativo móvel deve utilizar um modelo **local-first**:

```text
GPS / sensores
      ↓
Aplicativo
      ↓
Armazenamento local
      ↓
Fila de sincronização
      ↓
       API
      ↓
Servidor Tracker
```

Quando não houver conexão:

```text
Sem internet
     ↓
Dados permanecem no dispositivo
     ↓
Conectividade retorna
     ↓
Sincronização automática
```

A sincronização deve ser idempotente e tolerante a interrupções, evitando duplicação de pontos quando uma tentativa de envio for repetida.

---

# Frequência de coleta

A frequência de coleta deve ser configurável pelo usuário e, idealmente, pelo contexto da atividade.

Exemplos conceituais:

### Perfil econômico

```text
Localização: 5 min
GPS: baixa precisão
Sincronização: Wi-Fi
```

### Perfil normal

```text
Localização: 30 s
GPS: precisão normal
Sincronização: Wi-Fi + dados móveis
```

### Alta precisão

```text
Localização: 5 s
GPS: alta precisão
Sincronização: imediata
```

### Exercício

```text
Localização: 1 s
Velocidade: ativa
Altitude: ativa
Direção: ativa
```

A aplicação deve buscar um equilíbrio entre precisão, consumo de bateria, uso de dados e utilidade do histórico.

---

# Atividades

Uma atividade é uma unidade de contexto que agrupa dados relacionados.

Exemplo:

```text
Corrida
────────────────────────
Início: 06:30
Fim: 07:12

Distância: 6,82 km
Duração: 42 min
Velocidade média: 9,7 km/h
Velocidade máxima: 15,4 km/h

Elevação positiva: 82 m
Elevação negativa: 79 m

Pontos GPS: 1.284
```

O aplicativo deve apresentar essas informações em conjunto com o mapa do trajeto.

---

# Exercícios

O módulo de atividades físicas deve começar de forma simples e evoluir conforme novas fontes de dados forem suportadas.

### Caminhada

- distância;
- duração;
- velocidade;
- ritmo;
- altitude;
- trajeto.

### Corrida

- distância;
- pace;
- velocidade média;
- velocidade máxima;
- altitude;
- ritmo;
- trajeto.

### Ciclismo

- distância;
- velocidade média;
- velocidade máxima;
- elevação;
- tempo em movimento;
- trajeto.

No futuro, sensores externos poderão fornecer informações adicionais, como frequência cardíaca e cadência.

---

# Conteúdo multimídia

Atividades podem receber conteúdo associado:

```text
Activity
├── Track
├── Photos
├── Videos
├── Notes
└── Events
```

Uma mídia pode carregar metadados como:

- timestamp;
- latitude;
- longitude;
- atividade associada;
- dispositivo de origem.

O objetivo é permitir uma linha do tempo contextualizada, em vez de um simples álbum separado do histórico de localização.

---

# Recuperação de dispositivos perdidos

O Tracker deve possuir futuramente um modo específico para recuperação de dispositivos autorizados.

```text
Device
 │
 ├── Normal tracking
 │
 └── Lost Mode
```

Quando o proprietário ativar o modo de recuperação, o dispositivo poderá utilizar uma política de coleta mais frequente e priorizar a sincronização da localização.

O dashboard poderá apresentar:

```text
Device
────────────────────
Status: Online
Última localização: agora
Bateria: 63%
Velocidade: 12 km/h
Precisão: 7 m
Último movimento: agora
```

Essa função deve ser projetada para **recuperação de dispositivos próprios ou explicitamente autorizados**, sem mecanismos de rastreamento oculto ou furtivo.

---

# Privacidade

A privacidade deve estar presente tanto na arquitetura quanto na interface.

Exemplo de configurações:

```text
Privacy
────────────────────────
Precisão da localização
○ Exata
○ 10 m
○ 50 m
○ 100 m

Retenção do histórico
○ Permanente
○ 1 ano
○ 90 dias
○ 30 dias

Sincronização
☑ Wi-Fi
☑ Dados móveis

Rastreamento em segundo plano
☑ Ativado

Registro de atividades
☑ Ativado
```

Também está prevista a possibilidade de **zonas privadas**, permitindo que determinados locais tenham regras específicas de visualização ou compartilhamento.

---

# Segurança e controle de acesso

Cada dispositivo deve possuir uma identidade própria e credenciais independentes.

A arquitetura não deve depender somente de credenciais de usuário para autenticação de dispositivos.

Conceito:

```text
User
 ↓
Device
 ↓
Device Credential
 ↓
API
```

O sistema deve utilizar TLS para comunicação e autorização baseada em recursos no backend.

O frontend nunca deve ser considerado uma barreira de segurança: toda autorização deve ser validada no servidor.

---

# Multiusuário e RBAC

O Tracker deve suportar múltiplos usuários desde sua arquitetura inicial.

Um modelo inicial de permissões pode incluir:

```text
SUPER_ADMIN
    │
    ├── ADMIN
    │     │
    │     └── USER
    │
    └── SERVICE
```

A autorização deve considerar não somente o papel do usuário, mas também a propriedade dos recursos.

Exemplo:

```text
User A → seus Devices → seus Tracks → suas Activities
User B → seus Devices → seus Tracks → suas Activities
```

Isso prepara o projeto para uma arquitetura multi-tenant no futuro.

---

# Auditoria

Operações relevantes devem gerar registros de auditoria.

Exemplo:

```text
2026-09-10 03:01
User: user-123
Action: DEVICE_LOCATION_VIEW
Device: device-123

2026-09-10 03:02
User: user-123
Action: ACTIVITY_EXPORT
Activity: activity-9831

2026-09-10 03:03
User: admin-001
Action: USER_PERMISSION_CHANGED
User: user-456
```

A auditoria deverá contemplar, conforme o modelo final de segurança:

- autenticação;
- alterações de permissões;
- acesso a dados sensíveis;
- exportações;
- alterações de dispositivos;
- ativação do modo de recuperação;
- alterações de configurações relevantes.

---

# Stack tecnológica proposta

A stack abaixo representa a direção arquitetural inicial e poderá ser revisada durante a implementação.

| Componente | Tecnologia proposta |
|---|---|
| Backend/API | Go |
| Banco | PostgreSQL + PostGIS |
| Cache/fila | Redis, quando necessário |
| Web | React + TypeScript |
| Android | Kotlin / Android nativo |
| Desktop Agent | Go ou Rust |
| Containers | Docker / Docker Compose |
| Mídia | armazenamento compatível com S3 ou volume dedicado |

A escolha de Android nativo é especialmente relevante porque o rastreamento em segundo plano depende de APIs e políticas específicas do sistema operacional.

---

# Estrutura conceitual do projeto

```text
tracker/
├── api/
├── web/
├── worker/
├── mobile/
├── agent/
├── docker/
└── docs/
```

A estrutura definitiva será definida conforme os primeiros componentes forem implementados.

---

# MVP

O primeiro MVP deve ser deliberadamente menor que a visão final.

## Backend

- [ ] autenticação;
- [ ] usuários;
- [ ] dispositivos;
- [ ] credenciais de dispositivos;
- [ ] API de telemetria;
- [ ] PostgreSQL + PostGIS;
- [ ] armazenamento de pontos;
- [ ] auditoria básica.

## Android

- [ ] registro do dispositivo;
- [ ] coleta de localização em segundo plano;
- [ ] velocidade;
- [ ] altitude;
- [ ] direção;
- [ ] precisão;
- [ ] bateria;
- [ ] armazenamento offline;
- [ ] sincronização.

## Web

- [ ] dashboard;
- [ ] gerenciamento de dispositivos;
- [ ] mapa;
- [ ] histórico;
- [ ] atividades;
- [ ] configurações de privacidade.

## Primeira atividade

O MVP pode começar com uma atividade genérica de **deslocamento**, adicionando posteriormente:

- caminhada;
- corrida;
- ciclismo;
- viagem;
- transporte;
- atividades personalizadas.

---

# Roadmap conceitual

### Fase 1 — Foundation

- arquitetura;
- banco;
- autenticação;
- API;
- modelo de dispositivos;
- telemetria;
- auditoria.

### Fase 2 — Android

- coleta GPS;
- armazenamento local;
- sincronização;
- bateria;
- configuração de frequência.

### Fase 3 — Web

- dashboard;
- mapa;
- histórico;
- tracks;
- atividades.

### Fase 4 — Contexto

- classificação automática;
- lugares;
- geofences;
- multimídia;
- linha do tempo.

### Fase 5 — Exercícios

- corrida;
- caminhada;
- ciclismo;
- métricas;
- histórico esportivo.

### Fase 6 — Outros dispositivos

- desktop;
- notebook;
- trackers externos;
- API para dispositivos de terceiros.

### Fase 7 — Recovery

- Lost Mode;
- localização de dispositivo perdido;
- alertas;
- políticas específicas de coleta.

### Fase 8 — Cloud

- multi-tenancy;
- alta disponibilidade;
- backups;
- observabilidade;
- serviço hospedado pago.

---

# Self-hosted e serviço hospedado

A visão de distribuição do Tracker contempla duas modalidades.

## Community / Self-hosted

O usuário instala a plataforma em sua própria infraestrutura:

```text
Docker
├── tracker-api
├── tracker-web
├── tracker-worker
├── postgres
├── redis
└── object storage
```

Os dados permanecem sob controle do operador.

## Tracker Cloud

Uma futura versão hospedada poderá fornecer:

- alta disponibilidade;
- backups;
- armazenamento;
- atualizações;
- monitoramento;
- redundância;
- suporte.

A intenção é manter o mesmo núcleo de aplicação e evitar uma separação entre o produto self-hosted e o produto comercial.

---

# Visão de longo prazo

O objetivo do Tracker é evoluir de um simples coletor de GPS para uma **plataforma pessoal de histórico espacial e telemetria**.

```text
                    TRACKER
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   Localização      Movimento       Sensores
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                  Telemetria
                       │
                       ▼
                     Tracks
                       │
                       ▼
                   Activities
                       │
              ┌────────┼────────┐
              ▼        ▼        ▼
           Places    Media    Events
              │        │        │
              └────────┼────────┘
                       ▼
                  Timeline
                       │
                       ▼
                  Analytics
```

A proposta é transformar informações dispersas de dispositivos em um histórico consultável e controlado pelo próprio usuário.

---

# Status

🚧 **Projeto em fase de planejamento e definição arquitetural.**

Este README documenta a visão atual do projeto e serve como referência para as decisões de arquitetura e desenvolvimento do MVP.

As tecnologias, APIs, modelos de dados e componentes descritos como "propostos" ainda podem mudar durante a implementação.

---

# Contribuição

O projeto pretende ser open source e poderá receber contribuições conforme sua base técnica evoluir.

Discussões sobre arquitetura, privacidade, modelos de dados, sensores, consumo de bateria e interoperabilidade são especialmente relevantes para o desenvolvimento do projeto.

---

# Licença

A licença do projeto ainda será definida.

---

## Nota de privacidade

O Tracker foi concebido para coletar dados de localização e telemetria **com autorização do usuário**. Qualquer implementação de rastreamento deve respeitar consentimento, transparência, controle de acesso e legislação aplicável.
