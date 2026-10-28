# 5
Saga Snippets

In this chapter, we will be focusing on snippets in sagas. We will discuss an e-mail and **Secure File Transfer Protocol (SFTP)** example that will be set to timeout by a daily timer in saga code. The saga code will be a mediator between a frontend **Windows Presentation Framework (WPF)** and a backend client executing either e-mail or SFTP. Using a saga as a mediator between frontend and backend code that will interface into an external server will offer many added benefits and features. The external server interface, such as an e-mail server or SFTP server, is usually beyond our control and is in the control of external operations or organizations, such as a bank. So, the interface into these servers is all that we have to work with, and as business, software, and operational needs increase, we need a framework robust enough to meet these demands. Thus, we have NSB and sagas.

We will also walk through changing this application to support ActiveMQ. We will briefly discuss ActiveMQ and how to set it up to perform these operations.

In this chapter, we will cover:

+   - Sample e-mail saga notification
+   - Sample SFTP saga
+   - Saga deployment
+   - ActiveMQ