# 8

# Swift Object-Storage Service

In the previous chapter, we discussed the Cinder block-storage service and the Cinder-related objectives on the Certified OpenStack Administrator exam. Now we will discuss Swift. Swift is the OpenStack object-storage service, and it provides users with redundant cloud storage accessible via a RESTful API.

In this chapter, we will cover the following topics:

+   - Swift architecture
+   - Swift concepts
+   - Exam objective - managing containers and objects
+   - Exam objective - setting ACLs on a container
+   - Exam objective - setting objects to expire

After this chapter, you should have a solid understanding of Swift and the skills necessary to successfully fulfill all Swift-related objectives on the exam.

# About Swift

If you wanted to sign-up with a website hosting provider in the late 90's or early 2000's, you usually used a provider that would give you access to a server. That server would be allocated a particular amount of storage for you to use and store your files on. All for one monthly cost. You could upload your website's HTML pages, photos, and other static files via Secure SSH or an FTP client. If you needed additional storage, you would submit a request and the hosting provider would expand your capacity.