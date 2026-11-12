# How it works...

JavaMail API provides classes such as Session, Transport, Authenticator, and so on, that are used for composing, sending, and reading e-mails. If you want to directly use the JavaMail API to send an e-mail message, then you'll need to know how to use different JavaMail API classes and interfaces. Spring framework abstracts the inner workings of JavaMail API by providing a set of classes and interfaces, which simplifies writing programs that require the functionality of sending e-mails. Also, Spring provides an exception hierarchy, which abstracts the exceptions thrown during composing, parsing, and sending e-mails or while authenticating with the mail server.

The following figure shows the important classes and interfaces of Spring that provide e-mail sending functionality. You can find these classes and interfaces in the org.springframework.mail and org.springframework.mail.javamail packages of the Spring framework.

![](bbox=[0.308, 0.39, 0.686, 0.652])

MailSender interface is a generic interface that defines e-mail sending functionality. As shown in the given figure, it defines two send methods that accept Spring's SimpleMailMessage and SimpleMailMessage [] objects as arguments.

The JavaMailSender interface extends the MailSender interface and defines methods specific to sending e-mails using JavaMail API. JavaMailSender defines methods such as createMimeMessage—for creating a MimeMessage instance (which is part of JavaMail API), send—for sending a MimeMessage, and so on. For the complete list of methods defined by Spring, refer to the Spring API documentation for the JavaMailSender interface. Spring provides a concrete implementation of the JavaMailSender interface through the JavaMailSenderImpl class, which you can use in your application to send e-mails.