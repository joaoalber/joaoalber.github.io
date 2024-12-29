---
layout: post
title:  "How to build your own e-Sign service 🖊️💻"
date:   2023-11-29 14:00:00 -0300
categories: e-sign service ruby
---

You have surely come across an electronic signature service, such as the renowned _“DocuSign”_, haven’t you? 🤔

In case you’ve never heard of it, _DocuSign_ is an electronic signature service. You know those signatures you have to make when you’re renting/buying a property? So, these are the ones, but in a completely electronic/digital way.

Today, we will explore one of the countless ways to develop a **Back-End service that performs electronic signatures on specific documents.**

In my opinion, the two main benefits of using your own e-Sign service are: **lower costs and greater customization capabilities**

What is **essential** to start this process? 📝

*   Back-End Language (we will choose Ruby)
*   Some cloud storage service to save the files (such as GCP, AWS, among others)
*   Some libraries to carry out the necessary operations (I’ll get to this in a moment, let’s build some suspense 😅)

With these resources in hand, we will begin developing our e-Sign service.

The first step will be to structure the contract, which will be an HTML document (ignore HTML/CSS bad practices):

![HTML code demonstrating how the draft approach is](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*TvekvMcx0MyZtQYCauZsuQ.png)

Which will generate the following structure:

![HTML rendered example](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3kRJ_Wu-a89zZ0AFhFE5WQ.png)

So, we already have the contract that will be signed later. Note the presence of a **placeholder**, which will be essential for the effective signing of the document.

With this structure ready, the next step is to store the document in our file storage service, which basically consists of uploading the file to the cloud ☁️

In the following example, we use Ruby code to create the file in a **GCS (Google Cloud Storage) bucket**.

![Ruby code snippet showing how to connect with GCP](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*N9S9kOMnpU4eCQIQC86KIQ.png)

Now that we have the structure of our document stored in the cloud, it’s time to define two routes in our application 🛣️

The first is intended for **previewing the document**, accessible through a request: `GET users/{user_id}/documents/{document_id}/preview`

The second route will be used to **sign our document**, based on the request:  `POST users/{user_id}/documents/{document_id}/sign`

But there is still a problem: how to convert **HTML to PDF** and also perform the appropriate placeholder changes? A possible solution would be to add a common class for both routes.

This class would be responsible for replacing the placeholders and also **converting the document to PDF format**. An implementation for this in Ruby could be:

![Ruby code snippet showing how to conver a HTML file to a PDF one](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*W_usanKzO_axC7QhuibSpw.png)

This code is responsible for:

*   Download the HTML file that was saved in the cloud
*   Check whether the route called was **preview** or **sign** and, based on this, assign the necessary data to change the placeholders (**empty or the person’s full name**)
*   Transform HTML into a final document in PDF format

**3 libraries were used to perform the operations:**

*   Grover (turn HTML into PDF)
*   Mustache (exchange placeholders)
*   Google Cloud (interface with GCP)

**I used these libraries just because I was already used to them**, you can use the one that makes the most sense and/or is most convenient for you

After that, the signed document will look something like this:

![Ruby code snippet showing how to conver a HTML file to a PDF one](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Tgh3HYbrmIEFkt1lrkc4rA.png)

If you want to make it “more professional”, you can add a different font directly in the HTML to style the signature 💅

This solution is useful when the customer accepts the electronic signature terms of a certain product, so all they need to do is have their full name and sign with their consent.

Another possible **feature** would also be to add the possibility for the customer to create their own rubric, but that’s for another time 😅

> DONE!!! 🧙 It wasn’t that complex, right?

**The objective of this article was to show the solution to a common problem: digitally signing documents**. Note that some details have been intentionally omitted to keep the explanation as clear and accessible as possible.

Thank you very much for reading and see you around 👋😃
