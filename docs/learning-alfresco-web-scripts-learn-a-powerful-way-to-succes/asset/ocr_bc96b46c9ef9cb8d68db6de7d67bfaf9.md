The typical deployment architecture would be to have all the data web scripts hosted in the Alfresco repository server. The frontend clients using the backend repository can use presentation web scripts to build up the frontend user interface and presentation web scripts will be hosted at the client side. Frontend clients can invoke data web scripts through HTTP in order to interact with the backend repository. The Alfresco repository server will use Servlet Runtime to execute the web scripts.

# Summary

In this chapter, we learned the web script execution flow in detail along with its technical details. Also, we went through the different components that collectively make up the web script framework. We have also gone through the various out-of-the-box implementations available in Alfresco for these components and the most common deployment architecture of Alfresco web scripts.

As we have explored the web script framework in Alfresco, we are going to take a look at the various components in detail for building up web script in the next chapter.