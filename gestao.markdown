---
layout: page
title: Gestão
permalink: /gestao/
nav_order: 5
---

# Gestão

O envio de e-mails é acompanhado de certas responsabilidades, que o remetente deve gerenciar para manter uma taxa de entrega alta e uma boa reputação com os provedores de email.
## Reputação

A gestão da reputação é feita de 3 formas:

### Complaints

Complaints são incidentes onde um recipiente ativamente reporta uma mensagem como indesejada ou SPAM, geralmente essa ação é feita através de um botão como o "Reportar Spam" no seu cliente de email.

O número de complaints é acompanhado tanto pelo provedor de email do recipiente, podendo impactar na reputação. E também é acompanhados pelos provedores de envio de email, que usam essa métrica para acompanhar se um dos seus clientes está exercendo alguma atividade maliciosa.

É importante que o número de complaints seja baixo, e para isso, os provedores de envio normalmente fornecem diversas maneiras de consultar os complaints, seja através de uma interface web, relatório por email, ou por meio programáticos, como APIs e webhooks. A maioria dos provedores de envio tem uma tolerância muito baixa, logo, dependendo do seu volume de email, o monitoramento desses canais de forma automatizada é de primeira importância para a preservação de uma boa reputação.

### Bounces

Bounces são muito similares aos complaints, as diferenças são que os bounces possuem dois diferentes tipos, hard bounces e soft bounces. O hard bounce indica uma falha permanente na entrega, como um endereço de email inválido, já o soft bounce indica uma falha temporária na entrega, como uma caixa de email cheia, ou servidor temporariamente indisponível.

O número de bounces também é acompanhado de perto por provedores de envio e recebimento, que os usam para acompanhar possíveis atividades maliciosas.

Apesar dos provedores serem mais lenientes com os bounces, um número alto de bounces também pode levar a uma degradação da reputação do remetente ou até mesmo um bloqueio pelo provedor de envio. Os provedores também fornecem canais para acompanhamento de bounces, e normalmente são notificados juntamente com os complaints.

### Gerenciamento de bounces e complaints

Provedores de envio diferentes fornecem formas diferentes para o gerenciamento de bounes e complaints, aqui segue uma lista não exaustiva de como fazer a gestão de bounces e complaints de diversos provedores:

| Provedor                     | API         | Webhook                                     |
| :--------------------------- | :---------: | ------------------------------------------: |
|Postmark       |[API](https://postmarkapp.com/developer/api/bounce-api)                                                                                       |[Webhook](https://postmarkapp.com/developer/webhooks/bounce-webhook)|
|Twilio Sendgrid|-                                                                                                                                             |[Webhook](https://www.twilio.com/docs/sendgrid/ui/sending-email/bounces)|
|Amazon SES     |-                                                                                                                                             |[Webhook](https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity-using-notifications.html)|
|Mailgun        |[API](https://documentation.mailgun.com/docs/mailgun/api-reference/openapi-final/tag/Bounces/#tag/Bounces/operation/GET-v3--domainID--bounces)|[Webhook](https://www.mailgun.com/blog/email/your-guide-to-webhooks/#subchapter-1)|


### Inscrições

Uma boa gestão de listas de email é um dos porcessos essenciais para se manter uma boa reputação. Uma lista de emails com altas taxas de bounces e complaints pode ser um problema, para isso se faz necessário um monitoramento constante da lista, se aproveitando as técnicas mencionadas anteriormente como o gerenciamento de bounces e complaints para remover da lista de envios, seja de forma temporária ou permanente, os recipientes que apresentarem problemas.

Com o envio de emails transacionais não existe o problema de desinscrição de listas de marketing, porém, ainda é necessário prestar atenção em inúmeros outros eventos que podem ocorrer, pois a insistência do envio para endereços inválidos vai prejudicar a reputação do domínio.
