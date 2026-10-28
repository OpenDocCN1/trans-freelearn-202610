By the end of this chapter, you will be able to implement custom API endpoints, including token-based authentication.

# Technical requirements

All the code files of this chapter can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter12.

# Understanding REST APIs

Most modern web APIs can be classified as **REST** APIs. REST APIs are simply a type of API that focuses on communicating and synchronizing the *state* of objects between the database server and frontend client.

For example, imagine that you are updating your details on a website for which you are signed in to your account. When you go to the account details page, the web server tells your browser about the various details attached to your account. When you change the values on that page, the browser sends back the updated details to the web server and tells it to update these details on the database. If the action is successful, the website will show you a confirmation message.

This is a very simple example of what is known as **decoupled** architecture between frontend and backend systems. Decoupling allows greater flexibility and makes it easier to update or change components in your architecture. So, let’s say you want to create a new frontend website. In such a case, you don’t have to change the backend code at all, as long as your new frontend is built to make the same API requests as the old one.

REST APIs are *stateless*, meaning neither the client nor the server stores any states in between to do the communication. Every time a request is made, the data is processed, and a response is sent back without the protocol having to store any intermediate data. What this means is that the API is processing each request in isolation. It doesn’t need to store information regarding the session itself. This is in contrast to a stateful protocol (such as **Transmission Control Protocol (TCP)**), which maintains information regarding the session during its life.

So, a **RESTful web service**, as the name suggests, is a collection of REST APIs used to carry out a set of tasks. For example, if we develop a set of REST APIs for the Bookr application to carry out a certain set of tasks, we can call it a RESTful web service. Next, we will start working on DRF.

# Django REST framework

DRF is an open source Python library that can be used to develop REST APIs for a Django project. DRF has most of the necessary functionality built in to help develop APIs for any Django project. Throughout this chapter, we will be using it to develop APIs for our Bookr project. In the following section, we will install and configure DRF.