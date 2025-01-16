+++
title = "Como fazer seu próprio serviço de assinatura eletrônica 🖊️💻"
description = "Como fazer seu próprio serviço de assinatura eletrônica 🖊️💻"
date = 2023-11-29
draft = false

[taxonomies]
categories = ["projects"]
tags = ["ruby", "cloud", "project", "gcp", "sinatra"]

[extra]
lang = "en"
toc = true
comment = true
copy = true
math = false
mermaid = false
display_tags = true
truncate_summary = false
featured = false
reaction = false
+++

Introdução  
================

Certamente você já se deparou com algum serviço de assinatura eletrônica, tal como o renomado _“DocuSign”_, não é mesmo? 🤔

Caso você nunca tenha ouvido falar, _DocuSign_ é um serviço de assinaturas eletrônicas. Sabe aquelas assinaturas que você tem que fazer quando tá alugando/comprando um imóvel? Então, são essas, só que de maneira totalmente eletrônica/digital.

Hoje, vamos explorar uma das inúmeras maneiras de desenvolver um **serviço de Back-End que efetua assinaturas eletrônicas em documentos específicos.**

Na minha opinião, os dois principais benefícios ao utilizar um serviço próprio de assinatura são: **custos mais baixos e maior poder de personalização.**

&nbsp;

**O que é essencial para iniciar este processo?** 📝

*   Linguagem Back-End (optaremos pelo Ruby) + Sinatra (para definir as rotas da API)
*   Algum serviço de armazenamento em nuvem para salvar os arquivos (como _GCP_, _AWS_, entre outros)
*   Algumas bibliotecas para fazer as operações necessárias (aguarde que iremos chegar lá, deixa eu fazer o suspense 😅)

&nbsp;

Com esses recursos em mãos, daremos início ao desenvolvimento do nosso serviço de assinatura.

Estrutura HTML  
================

O primeiro passo será estruturar o contrato, que será um documento em HTML (ignorem as más práticas de HTML/CSS):

![Código HTML demonstrando como se aproxima o modelo](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*TvekvMcx0MyZtQYCauZsuQ.png)

Que gerará a seguinte estrutura:

![Exemplo do contrato renderizado em HTML](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3kRJ_Wu-a89zZ0AFhFE5WQ.png)

&nbsp;

Assim, já temos o contrato que será assinado posteriormente. Notem a presença de um **placeholder**, que será essencial para a assinatura efetiva do documento.

Com essa estrutura pronta, o próximo passo é armazenar o documento no nosso serviço de armazenamento de arquivos, que consiste basicamente em fazer o upload do arquivo para a nuvem ☁️

Armazenamento de Arquivos (GCP)  
================

No exemplo a seguir, utilizamos um código em Ruby para criar o arquivo em um **bucket do GCS (Google Cloud Storage).**

![Snippet de código Ruby mostrando como se conectar com o GCP](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*N9S9kOMnpU4eCQIQC86KIQ.png)

Rotas  
================

Agora que temos a estrutura do nosso documento armazenada na nuvem, é hora de definir duas rotas na nossa aplicação 🛣️

Se você não está familiarizado com o Sinatra, pode dar uma olhada rápida na [documentação oficial](https://www.rubydoc.info/gems/sinatra).

A primeira é destinada à **visualização do documento**, acessível através de uma requisição: `GET users/{user_id}/documents/{document_id}/preview`

A segunda rota será utilizada para a **assinatura do nosso documento**, a partir da requisição: `POST users/{user_id}/documents/{document_id}/sign`

Mas ainda tem um problema: como converter o **HTML em PDF** e também realizar as trocas adequadas do placeholder? Uma possível solução seria adicionar uma classe em comum para ambas as rotas.

HTML para PDF  
================

Esta classe teria a responsabilidade de substituir os placeholders e, também, converter o documento para o formato **PDF**. Uma implementação para isso em Ruby poderia ser:

![Snippet de código Ruby mostrando como converter um arquivo HTML para PDF](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*W_usanKzO_axC7QhuibSpw.png)

Esse código é responsável por:

*   Fazer o download do arquivo HTML que foi salvo na nuvem
&nbsp;

*   Verificar se a rota chamada foi a de **pré-visualização** ou de **assinatura** e, com base nisso, atribuir os dados necessários para trocar nos placeholders (**vazio** ou o **nome da pessoa**)
&nbsp;

*   Transformar o HTML em um documento versão final no formato PDF

Bibliotecas  
================

**Foram utilizadas 4 bibliotecas para realizar as operações:**

*   Grover (transformar o HTML em PDF)
&nbsp;

*   Mustache (realizar a troca dos placeholders)
&nbsp;

*   Google Cloud (realizar a interface com a GCP)
&nbsp;

*   Sinatra e suas dependências (para as rotas da aplicação)

**Eu usei essas bibliotecas apenas por já estar acostumado com elas**, você pode usar a que mais fizer sentido e/ou for mais conveniente pra você

Conclusão  
================

Depois disso, o documento assinado se parecerá com algo assim:

![Imagem do código Ruby mostrando a conversão de HTML para PDF](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Tgh3HYbrmIEFkt1lrkc4rA.png)

Caso queira deixar “mais profissional”, é possível adicionar uma fonte diferente diretamente no HTML para estilizar a assinatura 💅

Essa solução é útil para quando o cliente aceitar os termos de assinatura eletrônica de determinado produto, dessa forma, basta termos o nome completo e assinar a partir do consentimento dele.

Uma outra **feature** possível também seria adicionar a possibilidade do cliente fazer a própria rúbrica dele, mas isso fica pra outra hora 😅

&nbsp;

> PRONTO!!! 🧙 Não foi tão complexo assim, né?

&nbsp;

**O objetivo desse artigo foi mostrar a solução de um problema comum: a assinatura digital de documentos**. Perceba que alguns detalhes foram intencionalmente omitidos para manter a explicação mais clara e acessível possível.

&nbsp;

Caso queira me acompanhar de perto (não tão perto pra não se assustar): [https://www.instagram.com/codandonagringa/](https://www.instagram.com/codandonagringa/)

Muito obrigado pela leitura e nos vemos por aí 👋😃
