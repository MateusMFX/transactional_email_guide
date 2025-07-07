---
layout: page
title: Encadeamento e controle de envio
permalink: /encadeamento-e-controle-de-envio/
nav_order: 2
---

# Encadeamento e controle de envio

Controle de fluxo no envio de emails é o gerenciamento de quantas mensagens de email seu sistema envia. Provedores de email impõe limites em quantas mensagens podem ser enviadas por janelas de tempo segundo/minuto/hora/dia. Esses limites são colocados para evitar sobrecarga no servidor e grandes quantidades de SPAM.

## Janelas de envio

É importante que sejam limitados os envios por janelas de tempo, alguns provedores divulgam esses limites para quem faz envio de emails em massa:

[Google](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/)
[Microsoft](https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730)
[Yahoo!](https://senders.yahooinc.com/best-practices/)

Estabeler e controlar o número de mensagens que são enviadas numa janela de tempo pode ajudar a manter uma boa reputação, e evitar que suas mensagens caiam na caixa de SPAM.

Esse tipo de controle deve ser feito a nível de aplicação, e diversas técnicas podem ser utilizadas, a mais simples sendo calcular o limite diário de um provedor, dividir pela quantidade de segundos em um dia, e limitar o envio a tantas mensagens por segundo.

## Ordenação por domínio

Uma técnica mais complicada de controle de fluxo seria a ordenação de envio de mensagens por domínio, onde ao invés de enviar as mensagens diretamente na ordem em que são geradas, elas são acumuladas por um período de tempo, e após o acumulo, o envio é feito.

Aqui é sugerido que seja feito um controle por janela de envio como mencionado acima, e aliado a essa prática, seja feita uma alternância entre o envio para domínio, por exemplo:

Dada a lista:

```
alice@example.com
bob@example.com
carol@example.com
dave@example.com
eve@example.com
frank@testmail.com
grace@testmail.com
heidi@testmail.com
ivan@sample.org
judy@sample.org
```

Dadas as devidas proporções, fazer o envio na ordem apresentada pode acarretar na quebra do limite de envio para o domínio *@example.com*, caso seja utilizada uma técnica de cadenciamento de mensagens, também pode ocorrer um atraso nas entregas, caso os limites do primeiro domínio sejam muito baixos.
Para evitar esse problema, uma ordenação com alternância de domínios da lista pode ajudar:

```
alice@example.com
frank@testmail.com
bob@example.com
ivan@sample.org
carol@example.com
grace@testmail.com
dave@example.com
heidi@testmail.com
eve@example.com
judy@sample.org
```

Da forma listada acima, nenhum dos domínios vai receber mais de uma mensagem seguida, permitindo um maior fluxo de envio, e correndo menos risco de ultrapassar os limites impostos pelos domínios.

Numa lista com 10 endereços de email e 3 domínios diferentes isso não faria muita diferença, porém, em larga escala, com uma amostra grande o bastante, essa ordenação se faz muito importante.
