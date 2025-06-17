---
layout: page
title: Identidades
permalink: /identidades/
---

# Identidades
Identidade vai ser o termo utilizado para identificar um conjunto de credenciais e configurações para envio de e-mails. Uma identidade vai ser composta por um domínio de origem: @exemplo.com, um endereço de origem, por exemplo: naoresponda@exemplo.com, e um provedor de envio de e-mails, como [SendGrid](https://sendgrid.com/), [Postmark](https://postmarkapp.com/), [Amazon SES](https://aws.amazon.com/ses/), ou mesmo um servidor próprio.

## Preparação e aquecimento
Independente do tipo de provedor de envio escolhido, todos eles seguem passos bem similares para configuração e autenticação de uma identidade.

O primeiro é a preparação do domínio

### Domínio
A preparação do domínio é feito através do estabelecimento de um servidor de recebimento de emails, o estabelecimento do servidor é feito através da criação de registros de tipo MX no DNS do domínio em questão, onde os endereços de IP que estão preparados para receber emails em nome daquele domínio são listados como entradas, associados a prioridade que devem ser utilizados:

```
10 smtp2.example.com.
20 smtp3.example.com.
30 smtp4.example.com.
5 smtp1.example.com.
40 smtp5.example.com.
```

A configuração dos registros MX impacta diretamente na entregabilidade de emails, ajudando a provar a legitimidade do remetente. A ausência dos registros pode indicar um não interesse em recebe-los, o que leva a muitos provedores de caixa de entrada a rejeita-los. E serve como via para recebimento de notificações sobre os envios de e-mail, como notificações de bounces e complaints. <!-- Fonte: \cite{li2024bounce}. -->

## Autenticação de emails
A autenticação da identidade deve ser feita utilizando SPF, DKIM e DMARC. Todas as 3 formas de autenticação são feitas através da publicação de registros no DNS do domínio em questão.

### SPF

O SPF é o primeiro passo para a autenticação de emails, através dele é possível indicar quais endereços de IP estão autorizados a enviar emails em nome daquele domínio. Ele ajuda a prevenir envio indevido em nome daquele domínio, provendo assim mais uma camada de segurança e confiança, aumentando assim a entregabilidade. <!-- \cite{czybik2023lazy}. -->

Um exemplo de registro SPF:

```
"v=spf1 include:_spf.exemplo.com ~all"
```

O registro SPF é uma entrada do tipo *TXT*, que vai na raiz do domínio, ele consiste de uma entrada de texto, com as seguintes partes: 

```v=spf1``` indica a versão do protocolo utilizado, essa parte está sempre presente, e a versão é sempre 1

```include:_spf.exemplo.com``` cada entrada do tipo include indica os endereços de IP que estão autorizados a enviar em nome daquele domínio, para listar mais de um endereço, basta duplicar essa entrada, como no exemplo abaixo:

```
"v=spf1 include:_spf.exemplo.com include:_spf.outroexemplo.com ~all"
```

```~all``` vai sempre ao final da entrada, e indica o comportamento desejado quando uma mensagem de email não se adéqua as regras descritas anteriormente, são 4 os valores possíveis:

- ```+``` PASS: a mensagem deve ser aceita.
- ```-``` FAIL: a mensagem deve ser rejeitada.
- ```~``` SOFTFAIL: a mensagem pode ser rejeitada.
- ```?``` NEUTRAL: uma falha neutra, que normalmente indica uma implementação de testes.

### DKIM

O DKIM é outra peça essencial da autenticação de emails, o DKIM adiciona uma camada a mais de segurança na troca de emails, garantindo a integridade da mensagem através de técnicas de criptografia, permitindo que recipientes validem que o conteúdo da mensagem é genuíno, e que não foi alterado no meio do caminho. <!-- \cite{ragheb2023effectiveness, wang2022large}. -->
A configuração do DKIM é feita em 4 passos:

1. A escolha de um seletor.
2. Geração do par de chaves criptográficas.
3. Publicação da chave pública no DNS como um registro TXT usando o seletor escolhido.
4. Configuração do servidor de email para assinar as mensagens com a chave privada.

Um seletor é uma forma de identificar qual chave pública um destinatário deve utilizar para verificar o conteúdo da mensagem, exemplos: ***googleworkspace1*** ou ***microsoft3652***.

É possível configurar o DKIM manualmente, porém, a maioria dos provedores de serviços e email fornecem opções automáticas para configuração de DKIM, e eles também fornecem guias sobre como configura-los, Exemplos de provedores de caixa de entrada: [Google Workspace](https://support.google.com/a/answer/174124) e [Microsoft 365](https://learn.microsoft.com/en-us/defender-office-365/email-authentication-dkim-configure).

Exemplos de provedores de envio de email: [Postmark](https://postmarkapp.com/support/article/1091-how-do-i-set-up-dkim-for-postmark) [Sendgrid](https://sendgrid.com/en-us/blog/how-to-use-dkim-to-prevent-domain-spoofing)

Um exemplo de uma entrada DKIM publicada no DNS:

```
"v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4zd3nfUoLHWFbfoPZzAb8bvjsFIIFsNypweLuPe4M+vAP1YxObFxRnpvLYz7Z+bORKLber5aGmgFF9iaufsH1z0+aw8Qex7uDaafzWoJOM/6lAS5iI0JggZiUkqNpRQLL7H6E7HcvOMC61nJcO4r0PwLDZKwEaCs8gUHiqRn/SS3wqEZX29v/VOUVcI4BjaOz" "OCLaz7V8Bkwmj4Rqq4kaLQQrbfpjas1naScHTAmzULj0Rdp+L1vVyGitm+dd460PcTIG3Pn+FYrgQQo2fvnTcGiFFuMa8cpxgfH3rJztf1YFehLWwJWgeXTriuIyuxUabGdRQu7vh7GrObTsHmIHwIDAQAB"
```

O registro DKIM utiliza ponto e vírgula como delimitador de campos, e é composto das seguintes partes:

```v=DKIM1``` Indica que a versão do DKIM utilizada é a 1.

```k=rsa``` Indica que o tipo de critptografica utilizado é o RSA.

```p=MII...``` Indica que tudo após o ***p=*** é a chave pública.


### DMARC

A configuração do DMARC é o ultimo e mais importante passo para autenticação de emails. O DMARC exige que o SPF e o DKIM estejam configurados corretamente, aumentando a confiança do recipiente de que aquela é uma mensagem desejada. A recomendação é que o DMARC seja configurado em etapas, inicialmente com uma política de monitoramento, para que sejam encontradas e corrigidas todas as fontes de email, e eventualmente passando para uma política de bloquei. <!-- \cite{tatang2021evolution} -->

A configuração do DMARC também é feita através de uma entrada TXT no DNS, na entrada ```_DMARC```. Como no exemplo a seguir:

```
"v=DMARC1; p=reject; rua=mailto:mailauth-reports@example.com"
```

```v=DMARC1``` Indica que a versão do DMARC utilizada é a 1.

```p=reject``` Indica o tipo de política a ser utilizada, são 3 possibilidades, ***none*** que também é conhecida como a política de monitoramento, informa ao destinatário que nenhuma ação deve ser tomada, apenas relatórios devem ser enviados. ***quarantine*** que informa que a mensagem deve ser quarentenada, ou seja, enviada a caixa de SPAM. ***reject*** que informa que a mensagem deve ser rejeitada.

```rua=mailto:mailauth-reports@example.com``` Indica o endereço de email ao qual devem ser enviados os relatórios de entrega de emails.
