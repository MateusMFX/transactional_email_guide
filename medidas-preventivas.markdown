---
layout: page
title: Medidas Preventivas
permalink: /medidas-preventivas/
nav_order: 3
---

# Medidas Preventivas

Para manter uma boa reputação, também são necessárias outras medidas além da autenticação dos emails, neste capítulo abordaremos a prevenção de envio para endereços de email inválidos, abordando 3 estratégias.

## Verificação de registros MX
Para recepção de emails um domínio deve possuir registros MX configurados, para que os servidores de entrega de emails saibam para onde encaminhar as mensagens, um domínio sem um registro MX não pode receber emails, logo, quando preparando uma mensagem para entrega, é importante que seja verificado se existem registros MX atrelados ao domínio do destinatário. Essa verificação é feita através de uma consulta DNS, no exemplo abaixo é feita uma consulta ao domínio ***gmail.com*** usando a ferramenta [DIG](https://en.wikipedia.org/wiki/Dig_(command)).

```
$ dig MX gmail.com
...
;; ANSWER SECTION:
gmail.com.		1405	IN	MX	30 alt3.gmail-smtp-in.l.google.com.
gmail.com.		1405	IN	MX	10 alt1.gmail-smtp-in.l.google.com.
gmail.com.		1405	IN	MX	5 gmail-smtp-in.l.google.com.
gmail.com.		1405	IN	MX	20 alt2.gmail-smtp-in.l.google.com.
gmail.com.		1405	IN	MX	40 alt4.gmail-smtp-in.l.google.com.
...
```

## Verificação ortográfica e sintática
Outro problema a ser abordado são os erros de digitação, é muito comum encontrar endereços de email com uma letra trocada: ***hotmali.com*** ou com uma letra a menos: ***yaho.com*** com um domínio incompleto: ***gmail.*** e até mesmo com caracteres inválidos no endereço do usuário:

```john,doe@example.com```

Para evitar envios nesse tipo de situação, existem inúmeras ferramentas que ajudam a fazer a validação do endereço de email, e elas podem ser utilizadas como auxilio para prevenção de envio para endereços inválidos. A formação correta de um endereço de email é descrita na [RFC6854](https://datatracker.ietf.org/doc/html/rfc6854).

A validação pode ser feita por expressões regulares ou por diversas bibliotecas estão disponíveis para diversas:

| Biblioteca                   | Linguagem   | Link                                        |
| :--------------------------- | :---------: | ------------------------------------------: |
|validator                     |JavaScript   |<https://npmjs.com/package/validator>                  |
|email-validator               |Python       |<https://pypi.org/project/email-validator>             |
|Presente na biblioteca padrão |Ruby         |STDLIB           |
|email-validator               |PHP          |<https://github.com/egulias/EmailValidator>            |
|Apache Commons Validator      |Java         |<https://commons.apache.org/proper/commons-validator>  |
|Presente na biblioteca padrão |Go           |<https://pkg.go.dev/net/mail#ParseAddress>                  |
|email-address                 |Rust         |<https://crates.io/crates/email-address> |

# Gestão

- [Gestão]({{ site.baseurl }}/gestao)
