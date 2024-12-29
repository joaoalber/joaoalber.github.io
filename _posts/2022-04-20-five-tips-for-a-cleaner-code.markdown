---
layout: post
title:  "5 Tips for a Cleaner Code"
date:   2022-04-20 14:00:00 -0300
categories: cleancode ruby
---

Nowadays, the tech area has a huge dynamism and it means that developers are changing their current jobs more than ever.

Do we really write code to a machine? We should not forget that there are other people writing code in the same base source as ours.

There are a lot of opinions and structures that defines a code as a “good one”. It’s not necessary about the shortest code, but a better way to express what your code means.

&nbsp;

All examples below were made on [Ruby language](https://www.ruby-lang.org/pt/)

&nbsp;

1\. Naming Stuff
================

Why naming stuff matters? Imagine you are joining a new company and trying to discover what this piece of code in a movies context means:

&nbsp;

```ruby
def first_elements(array, count)
  array.take(count)
end
```

&nbsp;

Is it clear the intention of this piece of code? The only thing that we can assume is we are taking the first elements of an array, but what is this _array_? What does _count_ mean?

What about this:

&nbsp;

```ruby
def most_watched_movies(watched_movies, top_ranking)
  watched_movies.take(top_ranking)
end
```

&nbsp;

Better, right?

Naming things improves the communication between everyone who is coding in the same base source and as most explicit you are is better.

&nbsp;

2\. Improving Conditionals
==========================

We have some ways to improve the code conditionals, look at this:

&nbsp;

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

&nbsp;

Do you imagine a better way to do this _switch case_? It seems these values are supposed to be constants, so… What about this:

&nbsp;

```ruby
MOBILE_PHONE_BRAND = {
   'Samsung' => 'Galaxy',
   'Apple' => 'iPhone',
   'Xiaomi' => 'MI'
}
```

&nbsp;

We translated the entire _switch case_ into a _hash_, now it’s easier to understand.

The next one can save your life in many times, and it is recommended by [Ruby Style Guides](https://github.com/rubocop/ruby-style-guide) 😊

Have you ever heard about **early return** or **guard clause**?

&nbsp;

![Alt text](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ZUEXaTRBgZWkWSWcLHUCGw.png)

&nbsp;

Command sequence to perform hadouken

“Hadouken If-Else” — [MEME REFERENCE HERE](https://miro.medium.com/max/1400/1*mL04Mh-tDosU6_OlqexwyQ.jpeg):

&nbsp;

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

&nbsp;

When you put your eyes into this piece of code it’s hard to understand what is happening, isn’t it? Calm down, we can do it better:

&nbsp;

```ruby
def send_coins(user)
  return 'User invalid' unless user.valid?
  return 'User has not a coupon' unless user.has_coupon?
  return 'User is not premium' unless user.premium?

  # send coins
end
```

&nbsp;

The “Hadouken _if-else_” is over. All we did was splitting the cases into small pieces and ending the mess of the conditional decisions. It has a lot of benefits:

*   It’s easier to maintain
&nbsp;

*   Avoids cognitive overload
&nbsp;

*   Improves readability

&nbsp;

Believe me, if you try to improve conditionals, your life (and other’s) will be better

&nbsp;

3\. Error Handling
==================

Error handling is important to make things work, if you don’t do this properly, if some exception raises it will be harder to understand what is happening in a certain context.

&nbsp;

```ruby
def send_notification
  # do a HTTP request
end
```

&nbsp;

Imagine this situation, a critical notification to the context, what if some error occur in the HTTP request? In this case, it will raise some HTTP client exception (if the client deals with it) and will be hard to understand what is happening.

You should catch it and determine what you will do with the error:

&nbsp;

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

&nbsp;

💡 When you don’t specify the error in Ruby, it will rescue any error that extends _StandardError_ class, for more information check the [error hierarchy in Ruby](https://www.honeybadger.io/blog/understanding-the-ruby-exception-hierarchy/)

⚠️ **Never** rescue _Exception_, if you do it, **all** your system errors can be caught. The most specific your error is, the better to determine what you have to do, it’s your _fallback._

&nbsp;

4\. Avoiding Unnecessary Comments
=================================

It seems to be simple, isn’t it? Some days ago I was coding in a legacy code and I came across the following:

&nbsp;

```ruby
# C57
def something
   ...
end
```

&nbsp;

Ok, now tell me… You know what C57 means? I spent a little time to realize that it means NOTHING 😅

&nbsp;

![Alt text](https://miro.medium.com/v2/resize:fit:640/format:webp/1*JxoTBullcH8srYMZVkWd1w.png)

&nbsp;

Unnecessary comments, keep to yourself

In general, unnecessary comments can make confusion and time spent and nowadays, time is money…

&nbsp;

5\. Writing Better Tests
========================

&nbsp;

We all know that testing is a good thing, it can avoid a lot of _bugs_ and can be a documentation complement.

What about writing better tests?

When I start writing some test I put on my mind that I need to be clear because a lot of developers will use this test to understand the application rules. So, I follow the [Four-Phase Test Pattern](http://xunitpatterns.com/Four%20Phase%20Test.html):

Tests can be separated in four steps: _Arrange, Act, Assert_ and _Teardown._

&nbsp;

Which can be summarized in: preparing everything for the test, stimulating what will be tested, verifying that the result is what was expected and cleaning everything that was done during that test.

💡 In Ruby, when we are doing transactions on database, the _teardown_ occurs “automagically”, rollbacking every transaction in each final of the test

Example:

&nbsp;

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

&nbsp;

It’s easy to see every step of this unit test, splitting into minor pieces makes things less complicated.

&nbsp;

Conclusion
==========

These tips can guide you into your developer journey, your code will be cleaner and more important: will help other developers to understand your code.

&nbsp;

And don’t forget:

> Programmers must avoid leaving false clues that obscure the meaning of code.  
> —  Robert C. Martin

Thanks for reading 🍻
