

## Table of Contents

- Executing custom user scripts in EC2 66
- Writing the user script 66
- Providing a custom user script 67
- Removing the Chef node 70
- Interacting with Rackspace Cloud 70
- Provisioning a Rackspace instance 71
- Injecting configuration into the virtual machine 72
- Terminating the instance 74
- Removing the Chef node 75
- Summary 76
- Chapter 6: Going Beyond the Basics 77
- Chef's declarative language 77
- Handling multiple platforms 79
- Declaring support in your cookbook 79
- Multiplatform recipes 80
- Reducing complexity 82
- Versioning and source control 83
- Testing recipes 84
- RSpec and ChefSpec 84
- Testing basics 84
- RSpec 85
- ChefSpec 86
- Executing tests 87
- Understanding failure 88
- Expanding your tests 89
- Summary 90
- Index 91

## Preface

Welcome to *Managing Windows Servers with Chef*. This book is designed to familiarize
you with the concepts, tools, and features available to help you manage Windows
hosts with Chef. Inside the book, you will learn what you can expect from Chef on
Windows, how to get started using it, and what Chef provides for managing Windows
hosts that differs from Linux systems. Included are examples of deploying a complete
.NET/IIS application stack, cloud integration, and some information on developing
and testing for heterogeneous networks of Windows and Linux-based hosts.

## What this book covers

*Chapter 1, Chef and Windows*, serves as an introduction to Chef’s support for
Windows, what sort of features you can expect from Chef on the Windows platform,
and how to get started.

*Chapter 2, Installing the Client – an Overview of Chef Concepts*, provides coverage of
how to install the client on a Windows host as well as a quick overview of Chef’s
architecture and terminology and other important information to get you started
with managing Windows.

*Chapter 3, Windows-specific Resources*, introduces you to the resources that are unique
to managing Windows via Chef. This chapter provides descriptions and examples
of each resource, including roles, features, package installers, batch scripts, the
Windows registry, and many more.

*Chapter 4, Provisioning an Application Stack*, provides a hands-on guide to provisioning
a complete application stack (the .NET framework, IIS configuration, database server
installation, and so on), including defining roles, setting up configuration data,
installing requirements, and configuring the application.

Chapter 5, Managing Cloud Services with Chef, covers integrating with various cloud providers such as AWS, Rackspace Cloud, and Azure.

Chapter 6, Going Beyond the Basics, focuses on the integration of existing systems in heterogeneous networks, how to deal with recipes and multiple platforms, testing, organization, and publishing of recipes.

## What you need for this book

This book expects that you have access to some important components in order to be successful. In order to execute the examples in the book, the following prerequisites are needed:

- Access to a Chef server for managing your configuration; a self-hosted installation or a Chef-hosted account will work equally well
- A workstation where you can install software including knife (Windows or Linux host)
- A text editor of your choice

Additionally, if you wish to try out any of the cloud-computing examples, you will need an account with the cloud hosting providers you are trying out.

## Who this book is for

This book is designed for system administrators who have had some exposure to Chef, either casually or in-depth. It covers the Windows-specific facets of Chef and expects that you have a working installation of the Chef server available for you to use, either hosted or self-managed. A working knowledge of some programming language, preferably Ruby, will be needed to get the most out of the examples and to build your own recipes.

## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text, cookbook names, recipe names, scripts, database table names, folder names, filenames, file extensions, and pathnames are shown as follows: "In the same way that you would leverage `knife ssh` for Linux systems, `knife winrm` is available to execute commands remotely on a Windows host using the WinRM protocol."

A block of code is set as follows:
```
search(:node, 'role:web_server').each do |node|
  ip = node[:external_ip]
  firewall_rule "#{ip}" do
    source "#{ip}"
    action :allow
  end
end
```

Any command-line input or output is written as follows:
`knife ssh "role:mysql" "chef-client" --sudo -x ubuntu`

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "If you check the **Chef Client Service** box during the installation phase, the service will be set to run automatically."

Warnings or important notes appear in a box like this.

Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book – what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an e-mail to <feedback@packtpub.com>, and mention the book title via the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on **www.packtpub.com/authors**.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for all Packt books you have purchased
from your account at http://www.packtpub.com. If you purchased this book
elsewhere, you can visit http://www.packtpub.com/support and register to
have the files e-mailed directly to you.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes
do happen. If you find a mistake in one of our books—maybe a mistake in the text
or the code—we would be grateful if you would report this to us. By doing so,
you can save other readers from frustration and help us improve subsequent
versions of this book. If you find any errata, please report them by visiting
http://www.packtpub.com/submit-errata, selecting your book, clicking
on the errata submission form link, and entering the details of your errata.
Once your errata are verified, your submission will be accepted and the errata
will be uploaded on our website, or added to any list of existing errata, under
the Errata section of that title. Any existing errata can be viewed by selecting
your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media.
At Packt, we take the protection of our copyright and licenses very seriously. If you
come across any illegal copies of our works, in any form, on the Internet, please
provide us with the location address or website name immediately so that we
can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected
pirated material.

We appreciate your help in protecting our authors, and our ability to bring
you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem
with any aspect of the book, and we will do our best to address it.

## 1 Chef and Windows

If you are reading this book, the presumption is that you are already familiar with Chef, at least conceptually, and that you have some Windows-based systems you need to manage. If you have used Chef to manage some Linux-based systems, or have interacted with chef-solo to manage virtual machines, you will be ready to hit the ground running. If not, don’t worry—there are plenty of explanations and examples to make you feel right at home.

If you need help installing the Chef server, there are a number of other resources available to help you do that, or you can sign up for the free tier of hosted Chef to get you started right away without managing your own server.

In this chapter, we will cover the following topics:

- Working with Chef and Microsoft Windows
- Ways to integrate Chef and Windows
- Supported platforms and technologies

### Working with Windows

For those who are new to Chef, the client-side components of it are written in Ruby, a very popular language. Due to the cross-platform nature of Ruby, support for Windows is as straightforward as support for Linux and UNIX-like systems and has been around for quite some time now, dating back to the release of the **knife-windows** gem circa 2011.

