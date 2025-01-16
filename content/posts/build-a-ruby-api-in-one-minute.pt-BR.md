+++
title = "Construa uma REST API em Ruby (em 1 minuto) ⚡"
description = "Você pode construir APIs REST de forma rápida!"
date = 2025-01-16
draft = false

[taxonomies]
categories = ["projects"]
tags = ["ruby", "api", "projeto", "rest", "sinatra"]

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

Ruby é conhecido por sua simplicidade, desenvolvimento rápido e confiabilidade. Com suas bibliotecas e frameworks poderosos, você pode conseguir muito com pouco. Hoje, exploraremos o Sinatra, um framework leve que permite que você crie APIs rapidamente. Isso o torna perfeito para criar uma Prova de Conceito (POC) ou pequenos aplicativos prontos para produção.

### O que você vai precisar 📝

*   **Ruby** (versão mais recente recomendada)
&nbsp;

*   **Sinatra** e dependências (rackup e puma)
&nbsp;

*   **Postman** para testes
&nbsp;
 
Após [instalar o Ruby](https://www.ruby-lang.org/en/documentation/installation/), vamos instalar o Sinatra:

```shell
gem install sinatra rackup puma
```

Em seguida, pra verificar se o sinatra está funcionando, crie esse arquivo Ruby:

```ruby
# myapp.rb
require 'sinatra'

get '/' do
  'Hello world!'
end
```

E pra executar ele:

```shell
ruby myapp.rb
```

Visite: [http://localhost:4567](http://localhost:4567)

(Você deve ver uma página com *Hello World*)

Construindo a API REST de livros
================

Vamos criar uma API com algumas operações básicas para gerenciar livros.

Por que apenas algumas operações em vez de todas? Fica até o final — sem spoiler 🤐

### Estrutura do código

```ruby
require 'sinatra'
require 'json'

# In-memory storage for simplicity
books = []

def json_response(data, status = 200)
  content_type :json
  status status
  data.to_json
end
```

Aqui estamos definindo algumas coisas:

- **Armazenamento na memória** - Estamos usando um array para simplificar. Em produção, você deve usar um banco de dados real.
- **Método json_response** -  Lida com o "parse" do JSON focando no *content_type*, status HTTP e os dados em si

Agora, vamos construir nosso primeiro ponto de *endpoint*!

### POST /books

```ruby
# Create a new book
post '/books' do
  payload = JSON.parse(request.body.read, symbolize_names: true)
  new_book = { id: books.size + 1, title: payload[:title], author: payload[:author] }
  books << new_book
  json_response(new_book, 201)
end
```

Observe que adicionamos o endpoint `POST /books` que significa "criar um novo livro" e podemos ir de linha em linha pra entender o que tá acontecendo:

1. Transforma o payload JSON enviado pelo client em um objeto Ruby
2. Constrói um novo objeto de livro e o adiciona ao `books` array
3. É o mesmo que "inserir" um novo registro no banco de dados
4. Envia o livro criado como uma resposta JSON com status 201

Tranquilo, né?!

Agora vamos construir outro endpoint que vai ajudar a ver os livros criados

### GET /books

```ruby
# List all books
get '/books' do
  json_response(books)
end
```

Não há segredo nesse, já temos os dados que são um array de livros dentro da variável `books`

*Isso seria o mesmo que realizar uma consulta para trazer todos os registros de um banco de dados.*

Com esses dois pontos finais definidos, nosso código final fica assim:

```ruby
require 'sinatra'
require 'json'

# In-memory storage for simplicity
books = []

def json_response(data, status = 200)
  content_type :json
  status status
  data.to_json
end

# Create a new book
post '/books' do
  payload = JSON.parse(request.body.read, symbolize_names: true)
  new_book = { id: books.size + 1, title: payload[:title], author: payload[:author] }
  books << new_book
  json_response(new_book, 201)
end

# List all books
get '/books' do
  json_response(books)
end
```

Testando
================

Para testes, usaremos o Postman, que é um client HTTP que nos ajuda a executar as operações em nossos endpoints

1. Rodando o servidor

```
ruby myapp.rb
```

2. Verificando os livros existentes

Aqui você vai executar um `GET /books`. Inicialmente, a resposta será um array vazio:

![Example of a GET /books request](https://ucarecdn.com/cc665f95-8508-499e-8dca-60f65ddcdfb2/getbooks1.png)

3. Criando um novo livro

Envie uma request para `POST /books` com o *body* da imagem

![Example of a POST /books request](https://ucarecdn.com/ee9eeb3f-0e6f-4ee0-b9af-3112f8156044/postbook.png)

Resposta:

```json
{
    "id": 1,
    "title": "Harry Potter",
    "author": "J.K. Rowling"
}
```

4. Verificando a criação

Execute outra requisição `GET /books` pra ver se o livro foi adicionado:

![Example of GET /books working](https://ucarecdn.com/d5725f4b-a8d0-40de-8cd5-17ae6079e8e9/getbooksworking.png)

> VOILÁ!!! 🧙 O livro tá ai

Conclusão
================

Você construiu uma API simples em 1 minuto! Mas pera, cadê os outros endpoints?

**Desafio**: Adicionar endpoints para operações de *update*, *delete* e *show*. Teste-os usando Postman ou outro cliente HTTP.

Se você tiver alguma dúvida, pergunta ou sugestão, pode entrar em contato comigo através das minhas redes sociais listadas na [minha página inicial.](https://joaoalber.github.io/pt-BR)

Obrigado por ler — happy coding! 👋😃
