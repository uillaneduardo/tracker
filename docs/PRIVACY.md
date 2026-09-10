# Privacidade

## Princípio

O Tracker deve coletar apenas dados necessários à finalidade autorizada e oferecer controle compreensível ao usuário.

## Controles esperados

- ativar/desativar coleta;
- frequência de coleta;
- frequência de sincronização;
- precisão;
- Wi-Fi/dados móveis;
- rastreamento em segundo plano;
- retenção;
- exportação;
- compartilhamento;
- zonas privadas.

## Precisão

O sistema pode armazenar precisão real para fins autorizados e permitir que a apresentação/compartilhamento utilize precisão reduzida. Não assumir que arredondar coordenadas no frontend protege dados armazenados.

## Retenção

Políticas de retenção devem ser explícitas e executadas no backend. Se dados processados dependem de dados brutos, a política deve considerar essas dependências.

## Zonas privadas

Uma zona privada pode aplicar regras de ocultação, redução de precisão ou exclusão conforme a política definida pelo produto. A implementação deve impedir que uma simples consulta alternativa revele a localização protegida.

## Exportação e exclusão

Exportações devem ser autenticadas, autorizadas e auditadas. Exclusões devem respeitar a política de retenção e dependências entre raw, processed, context e media.

## Consentimento

A instalação deve deixar claro quem controla o servidor e quais dados são coletados. O aplicativo não deve induzir o usuário a acreditar que uma função está desligada quando o servidor ainda recebe dados.

## Uso autorizado

O produto é destinado ao rastreamento de dispositivos próprios ou explicitamente autorizados. Recursos de recuperação devem ser transparentes e não devem oferecer mecanismos de rastreamento furtivo.

## Compliance

O projeto deve permitir adaptação às leis aplicáveis à instalação e ao contexto de uso. Requisitos legais específicos devem ser validados conforme jurisdição e finalidade antes de uma oferta comercial.
