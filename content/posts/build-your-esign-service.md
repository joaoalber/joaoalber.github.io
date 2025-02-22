+++
title = "How to build your own e-Sign service 🖊️💻"
description = "Have you imagined your own DocuSign?"
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


Introduction
================

You have surely come across an electronic signature service, such as the renowned _“DocuSign”_, haven’t you? 🤔

In case you’ve never heard of it, _DocuSign_ is an electronic signature service. You know those signatures you have to make when you’re renting/buying a property? So, these are the ones, but in a completely electronic/digital way.

Today, we will explore one of the countless ways to develop a **Back-End service that performs electronic signatures on specific documents.**

In my opinion, the two main benefits of using your own e-Sign service are: **lower costs and greater customization capabilities**

&nbsp;

What is **essential** to start this process? 📝

*   Back-End Language (we will choose Ruby) + Sinatra (for defining the API/routes)
&nbsp;

*   Some cloud storage service to save the files (such as GCP, AWS...)
&nbsp;

*   Some libraries to carry out the necessary operations (I’ll get to this in a moment, let’s build some suspense 😅)

&nbsp;

With these resources in hand, we will begin developing our e-Sign service.

HTML Structure
================

The first step will be to structure the contract, which will be an HTML document (ignore HTML/CSS bad practices):

![HTML code demonstrating how the draft approach is](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*TvekvMcx0MyZtQYCauZsuQ.png)

Which will generate the following structure:

![HTML rendered example](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3kRJ_Wu-a89zZ0AFhFE5WQ.png)

&nbsp;

So, we already have the contract that will be signed later. Note the presence of a **placeholder**, which will be essential for the effective signing of the document.

With this structure ready, the next step is to store the document in our file storage service, which basically consists of uploading the file to the cloud ☁️

File Storage (GCP)
================

In the following example, we use Ruby code to create the file in a **GCS (Google Cloud Storage) bucket**.

![Ruby code snippet showing how to connect with GCP](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*N9S9kOMnpU4eCQIQC86KIQ.png)

Routes
================

Now that we have the structure of our document stored in the cloud, it’s time to define two routes in our application using Sinatra 🛣️

If you are not familiar with Sinatra you can take a quick look at [this documentation](https://www.rubydoc.info/gems/sinatra)

The first is intended for **previewing the document**, accessible through a request: `GET users/{user_id}/documents/{document_id}/preview`

The second route will be used to **sign our document**, based on the request: `POST users/{user_id}/documents/{document_id}/sign`

But there is still a problem: how to convert **HTML to PDF** and also perform the appropriate placeholder changes? A possible solution would be to add a common class for both routes.

HTML to PDF
================

This class would be responsible for replacing the placeholders and also **converting the document to PDF format**. An implementation for this in Ruby could be:

![Ruby code snippet showing how to convert a HTML file to a PDF one](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*W_usanKzO_axC7QhuibSpw.png)

This code is responsible for:

*   Download the HTML file that was saved in the cloud
&nbsp;

*   Check whether the route called was **preview** or **sign** and, based on this, assign the necessary data to change the placeholders (**empty or the person’s full name**)
&nbsp;

*   Transform HTML into a final document in PDF format

Libraries
================

**4 libraries were used to perform the operations:**

*   Grover (turn HTML into PDF)
&nbsp;

*   Mustache (exchange placeholders)
&nbsp;

*   Google Cloud (interface with GCP)
&nbsp;

*   Sinatra and its dependencies (app routing)

**I used these libraries just because I was already used to them**, you can use the one that makes the most sense and/or is most convenient for you

Conclusion
================

After that, the signed document will look something like this:

![Ruby code snippet showing how to convert a HTML file to a PDF one](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Tgh3HYbrmIEFkt1lrkc4rA.png)

If you want to make it “more professional”, you can add a different font directly in the HTML to style the signature 💅

This solution is useful when the customer accepts the electronic signature terms of a certain product, so all they need to do is have their full name and sign with their consent.

Another possible **feature** would also be to add the possibility for the customer to create their own rubric, but that’s for another time 😅

&nbsp;

> DONE!!! 🧙 It wasn’t that complex, right?

&nbsp;

**The objective of this article was to show the solution to a common problem: digitally signing documents**. Note that some details have been intentionally omitted to keep the explanation as clear and accessible as possible.

&nbsp;

Thank you very much for reading and see you around 👋😃
