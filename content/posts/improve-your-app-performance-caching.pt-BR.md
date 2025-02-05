+++
title = "Caching: Como Melhorar a Performance da sua Aplicação (com Redis)"
description = "Um guia rápido sobre como utilizar caching em web apps"
date = 2025-02-05
draft = false

[taxonomies]
categories = ["performance"]
tags = ["caching", "redis", "ruby", "performance"]

[extra]
lang = "pt-BR"
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

Caching é a prática de armazenar resultados já calculados para evitar a necessidade de cálculos subsequentes, economizando tempo e recursos computacionais 🚀

Existem várias formas de implementar caching, desde o uso de variáveis em memória até soluções mais robustas e confiáveis, como bancos de dados dedicados. Esses serviços gerenciam e armazenam os dados de maneira eficiente.

Um dos sistemas de caching mais populares é o Redis, e é esse que vamos utilizar neste guia.

Vamos explorar na prática como implementar o caching e aplicar essa técnica em ambientes de produção. Embora usaremos Ruby para facilitar a explicação, os conceitos podem ser aplicados em qualquer linguagem.

Configuração
================

### O que você vai precisar 📝

*   Linguagem de Programação (utilizaremos Ruby)
&nbsp;

*   Docker (para rodar o servidor do Redis) 🐋
&nbsp;
 
Agora que você já tem o Docker configurado e instalado na sua máquina, vamos rodar o seguinte comando:

```shell
docker run -d -p 6379:6379 -i -t redis
```

Esse comando inicia o servidor Redis usando a versão mais recente da imagem oficial.

Após a execução, podemos verificar se o servidor está rodando com:

```shell
docker ps
```

O resultado esperado será algo semelhante a isto:

```shell
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
986bb85320f8   redis     "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   focused_jang
```

Observe que a coluna `STATUS` deve conter a palavra `Up`, indicando que o servidor está em funcionamento.

Criando o arquivo de caching
================

Agora, vamos criar um arquivo em Ruby para implementar o caching. Escolhemos Ruby pela didática, mas você pode usar qualquer outra linguagem.

### Estrutura do código

```ruby
require "redis"

CACHE_KEY_ID = "my-cache" 
REDIS = Redis.new(host: "127.0.0.1", port: 6379)

stored_value = REDIS.get(CACHE_KEY_ID)

if stored_value.nil?
  stored_value = Net::HTTP.get(URI("https://www.example.com"))
  REDIS.set(CACHE_KEY_ID, stored_value, ex: 3600)
end

stored_value
```

### Explicação do código

#### Inicialização e configuração:

```ruby
require "redis"

CACHE_KEY_ID = "my-cache" 
REDIS = Redis.new(host: "127.0.0.1", port: 6379)
```

- Importamos a biblioteca `redis-rb`, que permite a comunicação com o Redis. Em Python, por exemplo, a biblioteca equivalente seria `redis-py`
- Definimos `CACHE_KEY_ID`, que será a chave usada para armazenar e recuperar os dados no cache. Essa chave funciona como um identificador único, semelhante a um ID em um banco de dados
- Criamos a instância do Redis, configurando o IP (`127.0.0.1`) e a porta (`6379`), que são os mesmos valores usados ao rodar o servidor no Docker

#### Lógica de caching:

```ruby
stored_value = REDIS.get(CACHE_KEY_ID)

if stored_value.nil?
  stored_value = Net::HTTP.get(URI("https://www.example.com"))
  REDIS.set(CACHE_KEY_ID, stored_value, ex: 3600)
end

stored_value
```

Aqui realizamos duas operações principais: leitura e escrita no cache.

1. Verificação do cache

- Primeiro, tentamos obter o valor armazenado no Redis.
- Se o valor **já existir**, ele será utilizado e evitamos fazer uma nova requisição.

2. Armazenamento do valor

- Caso o valor não esteja armazenado (`nil`), buscamos a página HTML fazendo uma requisição `GET`
- Em seguida, armazenamos o conteúdo no Redis com um tempo de expiração de 1 hora (`ex: 3600`)
- Assim, novas requisições dentro desse período reutilizarão o valor salvo, evitando acessos desnecessários ao site externo

3. Retorno do valor

- No final, retornamos `stored_value`, que conterá o valor do cache ou, se necessário, o conteúdo recém-buscado.

## Benefícios 🚀

No nosso exemplo, o uso de cache melhora significativamente o desempenho da aplicação, pois:

- Evita requisições repetidas para serviços externos, reduzindo o tempo de resposta
- Garante que os dados sejam acessados mais rapidamente (experiência do usuário)

Essa estratégia permite que a aplicação responda de forma muito mais eficiente aos consumidores.

Usos do Caching
================

✅ Quando Usar

- Dados que mudam pouco (ex.: listas de produtos, configurações)
- APIs externas com limite de requisições (ex.: taxas de câmbio, previsão do tempo)
- Consultas pesadas no banco (ex.: relatórios, dashboards)
- Dados acessados frequentemente por muitos usuários (ex.: banners promocionais)

❌ Quando Não Usar

- Dados que mudam constantemente (ex.: status de pedidos, chats em tempo real)
- Quando a precisão é essencial (ex.: saldo bancário, estoque)
- Dados sensíveis ou temporários (ex.: tokens de autenticação)

Conclusão
================

Hoje você aprendeu uma tecnologia MUITO importante em pouco tempo de leitura! Agradeço por ler até aqui.

Se você tiver alguma dúvida, pergunta ou sugestão, não hesite em entrar em contato 😊

> REDIS.set("novo-aprendizado-em-caching", this.post)

Obrigado pela leitura!! 🍻
