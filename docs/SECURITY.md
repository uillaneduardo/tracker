# Segurança

## Princípios

O Tracker manipula localização, histórico e potencialmente mídia pessoal. Segurança deve ser tratada como requisito central.

## Identidade

- Usuários possuem autenticação própria.
- Dispositivos possuem identidade e credencial próprias.
- Credenciais podem ser revogadas/rotacionadas.
- Segredos não entram no repositório.

## Autorização

A API deve verificar em cada operação:

1. identidade do ator;
2. permissão para a ação;
3. ownership/ACL do recurso;
4. escopo do tenant quando existir.

Nunca confiar em IDs enviados pelo frontend para inferir autorização.

## Transporte

Produção deve utilizar TLS. Comunicação interna pode ser adaptada ao ambiente, mas não deve permitir exposição desnecessária de serviços.

## Dados sensíveis

Localização, histórico, dispositivos e mídia devem ser tratados como dados sensíveis. Logs não devem registrar conteúdo sensível desnecessariamente.

## Dispositivo perdido

Lost Mode deve ser explicitamente ativado por proprietário/administrador autorizado. Não implementar coleta secreta, bypass de consentimento ou mecanismo de vigilância furtiva.

## Auditoria

Registrar pelo menos autenticação relevante, alteração de permissões, acesso/exportação de dados sensíveis, gestão de dispositivos e ativação de recuperação.

## Desenvolvimento seguro

- validar entrada;
- usar queries parametrizadas/ORM seguro;
- limitar payloads;
- rate limiting na ingestão e autenticação;
- proteger endpoints administrativos;
- validar uploads e MIME types;
- evitar SSRF ao processar URLs externas;
- não expor stack traces/segredos em produção;
- testar isolamento entre usuários.

## Threat model inicial

Ameaças prioritárias:

- roubo de credencial de usuário;
- credencial de dispositivo comprometida;
- acesso cruzado entre usuários;
- replay/duplicação de telemetria;
- manipulação de timestamp/coordenadas;
- acesso indevido a mídia;
- exposição acidental em logs/exports;
- comprometimento do servidor self-hosted.

O threat model deve evoluir junto com o sistema.
