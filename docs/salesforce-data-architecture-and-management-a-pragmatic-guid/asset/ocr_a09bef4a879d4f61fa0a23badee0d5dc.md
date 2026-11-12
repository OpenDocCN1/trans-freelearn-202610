![](bbox=[0.12, 0.09, 0.84, 0.33])

Figure 2.12 – Creating a master-detail relationship

We can define a master-detail relationship with different options as seen above by specifying what level of access to the master record is required in order to create, edit, or delete detail records.

# Junction objects

Remember that parent-child and lookup relationships allow us to define one-to-many relationships, which means that there is one parent record and multiple child records, but what if we have a need to define a many-to-many relationship where we have many parent records associated with many child records. This is where a Junction object comes in.

A Junction object is a special type of object that lets us define a many-to-many relationship using two parent-child relationships on the object. In our recruitment app, we have a Job object and a Candidate object. The Job object is used to post new jobs, and the Candidate object is used to store information about candidates that are applying for a job. If we use a parent-child or lookup relationship, our candidate can apply for a single job at a time, which is not practical.

Our business requirement is that a candidate must be able to apply for multiple jobs at a time. For that, we will need another object, called Applicant, which will be a child to the Job and Candidate objects. The Applicant object will allow the candidate to apply for multiple jobs at a time. In other words, multiple jobs can be associated with multiple candidates and multiple candidates can be associated with multiple jobs via the Applicant object:

![](bbox=[0.19, 0.8, 0.77, 0.85])

Figure 2.13 – Applicant is a Junction object. In the user interface (UI), you will notice on the Applicant record one field each to enter the Job and Candidate data