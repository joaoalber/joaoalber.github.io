+++
title = "5 Dicas para um Código mais Limpo"
date = 2022-04-20
draft = false

[taxonomies]
categories = ["clean code"]
tags = ["clean code", "ruby", "teste", "rspec"]

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

Atualmente, a área de tecnologia tem um enorme dinamismo e isso significa que os desenvolvedores estão mudando seus empregos mais do que nunca.

Nós realmente escrevemos código para uma máquina? Não devemos esquecer que existem outras pessoas escrevendo código na mesma base de código que a nossa.

Existem muitas opiniões e estruturas que definem um código como “bom”. Não é necessariamente o código mais curto, mas uma maneira melhor de expressar o que seu código significa.

Todos os exemplos abaixo foram feitos na [linguagem Ruby](https://www.ruby-lang.org/pt/)

1\. Nomeando as Coisas
================

Por que nomear as coisas é importante? Imagine que você está entrando em uma nova empresa e tentando descobrir o que significa esse trecho de código em um contexto de filmes:

```ruby
def first_elements(array, count)
  array.take(count)
end
```

É clara a intenção do código? A única coisa que podemos assumir é que estamos pegando os primeiros elementos de um _array_, mas o que é esse _array_? O que significa _count_?

E agora:

```ruby
def most_watched_movies(watched_movies, top_ranking)
  watched_movies.take(top_ranking)
end
```

Melhor, certo?

Nomear as coisas melhora a comunicação entre todos que estão “codando” na mesma base de código e quanto mais explícito você for é melhor.

2\. Melhorando Condicionais
================

Temos algumas maneiras de melhorar as condicionais do código, por exemplo:

```ruby
case mobile_phone_brand
when 'Samsung'
  'Galaxy'
when 'Apple'
  'iPhone'
when 'Xiaomi'
  'MI'
end
```

Você imagina uma maneira melhor de fazer esse _switch case_? Parece que esses valores deveriam ser constantes, então… Que tal isso:

```ruby
MOBILE_PHONE_BRAND = {
   'Samsung' => 'Galaxy',
   'Apple' => 'iPhone',
   'Xiaomi' => 'MI'
}
```

Traduzimos todo o _switch case_ em um _hash_, e agora ficou mais fácil de entender.

O próximo pode salvar sua vida em muitas vezes, e é recomendado pelo [Ruby Style Guides](https://github.com/rubocop/ruby-style-guide) 😊

Você já ouviu falar sobre **early return** ou **guard clause**?

&nbsp;

![Alt text](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ZUEXaTRBgZWkWSWcLHUCGw.png)

&nbsp;

“If-Else Hadouken” — [REFERÊNCIA DO MEME AQUI](https://miro.medium.com/max/1400/1*mL04Mh-tDosU6_OlqexwyQ.jpeg):

```ruby
def send_coins(user)
   if user.valid?
     if user.premium?
       if user.has_coupon?
         # send coins
       else 
         'User has not a coupon'
       end
     else
       'User is not premium'
     end
   else
     'User invalid'
   end
end
```

Quando você se depara com esse trecho de código é difícil entender o que está acontecendo, não é? Calma, podemos melhorar:

```ruby
def send_coins(user)
  return 'User invalid' unless user.valid?
  return 'User has not a coupon' unless user.has_coupon?
  return 'User is not premium' unless user.premium?

  # send coins
end
```

O “I_f-Else_ Hadouken” acabou. Tudo o que fizemos foi dividir as etapas em pequenos pedaços e acabar com a bagunça das condicionais. E tem muitos benefícios:

*   É mais fácil de manter
*   Evita o esforço cognitivo
*   Melhora a legibilidade

Acredite, se você tentar melhorar as condicionais, sua vida (e a dos outros) será melhor

3\. Tratamento de Erros
================

O tratamento de erros é importante para fazer as coisas funcionarem, se você não fizer isso corretamente, se alguma exceção surgir, será mais difícil entender o que está acontecendo em determinado contexto.

```ruby
def send_notification
  # do a HTTP request
end
```

Imagine essa situação, existe uma notificação crítica pro contexto, e se ocorrer algum erro na requisição HTTP? Nesse caso, ele lançará alguma exceção do cliente HTTP (se o cliente lidar com isso) e será difícil entender o que está acontecendo.

Você deve capturá-lo e determinar o que fará com o erro:

```ruby
def send_notification
  begin
    # do a HTTP request
  rescue StandardError => error
    # do something with this error
    # could render, retry, log or send it to a monitoring system
  end
end
```

{% tip(header="") %}
  Quando você não especifica o erro em Ruby, ele irá capturar qualquer erro que estenda a classe _StandardError_, para mais informações verifique a [hierarquia de erros em Ruby](https://guru-sp.github.io/tutorial_ruby/excecoes.html)
{% end %}

{% warning(header="") %}
  **Nunca** especifique o erro _Exception_ ao capturar, se você fizer isso, **todos** os erros do seu sistema podem ser capturados. Quanto mais específico for o seu erro, melhor para determinar o que você precisa fazer, é o seu _fallback_.
{% end %}

4\. Evitando Comentários Desnecessários
================

Parece ser simples, não é? Alguns dias atrás eu estava “codando” em um sistema legado e me deparei com o seguinte:

```ruby
# C57
def something
   ...
end
```

Beleza, agora me fala… Você sabe o que significa C57? Passei um tempinho pra perceber que não significa NADA 😅

![Alt text](https://miro.medium.com/v2/resize:fit:640/format:webp/1*JxoTBullcH8srYMZVkWd1w.png)

Em geral, comentários desnecessários podem causar confusão e gastar tempo e, hoje em dia, tempo é dinheiro…

5\. Melhorando os Testes
================

Todos sabemos que testar é uma coisa boa, os testes podem evitar muitos _bugs_ e ser um complemento para a documentação

Mas e escrever bons testes?

Quando começo a escrever algum teste, penso que preciso ser claro porque muitos desenvolvedores usarão esse teste para entender as regras da aplicação. Então, eu sigo o padrão [Four-Phase Test](http://xunitpatterns.com/Four%20Phase%20Test.html):

Testes podem ser separados em quatro etapas: _Arrange, Act, Assert_ e _Teardown._

Que podem ser resumidos em: preparar tudo para o teste, estimular o que será testado, verificar se o resultado é o esperado e limpar tudo o que foi feito durante aquele teste.

{% tip(header="") %}
  Em Ruby, quando estamos fazendo transações no banco de dados, o _teardown_ ocorre “automagicamente”, fazendo um _rollback_ a cada transação durante o fim do teste
{% end %}

Exemplo:

```ruby
describe "#perform" do
  it "sends event to Sentry" do   
    # Arrange
    event = { message: "an error event" }
    worker = described_class.new
    allow(Raven).to receive(:send_event).with(event)
      
    # Act
    worker.perform(event)

    # Assert
    expect(Raven).to have_received(:send_event).with(event)
  end
end
```

É fácil ver cada etapa do teste de unidade, dividir em partes menores torna as coisas menos complicadas.

Conclusão
================

Essas dicas podem guiá-lo na sua jornada de desenvolvedor, seu código ficará mais limpo e mais importante: ajudará outros desenvolvedores a entender seu código.

E não se esqueça:

> _Os programadores devem evitar deixar pistas falsas que obscureçam o significado do código._ (tradução literal)— Robert C. Martin

Obrigado pela leitura!! 🍻

