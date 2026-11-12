This is an expected question. How are you planning to handle it? If you want to ensure that you collect all points, you must draw the sequence diagram and explain it. You learned about these two flows in detail in Chapter 4, Core Architectural Concepts: Identity and Access Management. The diagram will not be repeated in this chapter. Make sure to memorize all the authentication flows covered in this book.

Explore one last question raised by the judges:

*PPA wants to change the registration process a bit. They want to include a copy of the driving license with the online registration request. The request should be assigned to one of the support agents to validate the driving license’s image. The customer can access the community after the support agent approves the registration request. How would you adjust your solution to meet this requirement?*

This is one of the requirements that test your ability to solution on the fly. At the same time, it tests your platform knowledge. Your answer could be as follows:

*I will introduce an additional mandatory step during the customer registration process to request uploading the driving license image. I will also change the code of the registration’s Lightning component so that it creates a Case record of a specific type in addition to the Community user and Person account. The code will also associate the Case record with the newly created Person account.*

*I am aware that I need to use a Future or a Queueable class to avoid the mixed DML exception while creating the Case record. The Case record will be automatically assigned to the right queue based on the configured Case **Assignment Rules.***

*Once the Case is resolved/approved, I will update a flag on the community user’s record to indicate that it has been validated. I will utilize a Custom Login Flow to prevent the newly created Community users from logging in until they get that flag set to true.*

That concludes the Q&A stage. You experienced different types of questions that could be raised and confidently defended your solution or adjusted it if needed. Try to keep your answers short, concise, and to the point. This will give more time for the judges to cover topics that you might have missed.