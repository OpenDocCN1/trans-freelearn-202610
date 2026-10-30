
We should see output similar to that shown in Figure 8.2. Now let the process run and open a new terminal, where we need to run the `python manage.py runserver` command. Open the `/verify` endpoint in the browser by going to `http://127.0.0.1:8000/blog/verify/?verify_word=abc`. We should get a success response on the browser, and when we go to the Celery worker terminal, we should see output reading `Celery Task verification: abc`, similar to what is shown in Figure 8.3:

![Figure 8.3: Output from a Celery task that was initiated in Django](image/Figure_8.3_B18867.jpg)

Figure 8.3: Output from a Celery task that was initiated in Django

### Important note

Celery doesn’t support hot reloading out of the box. It is common for developers to face a problem where they have updated the code, but the task is not working as expected in the local development server. Please remember that when we start the Celery worker, it loads the code in memory and keeps running it unless we manually restart the Celery worker.

To solve this problem, we can use third-party packages such as Watchdog and create custom commands. Read more about Watchdog at [https://github.com/gorakhargosh/watchdog](https://github.com/gorakhargosh/watchdog).

We have integrated Celery with Django. Let us now learn how we can effectively use Celery in a Django project and examine the different interfaces Celery provides.

## Interfaces of Celery

Celery is useful to execute any long-running tasks that Django applications need to run. Whenever Django encounters any long-running/background job to be executed, it lets Celery work on that task instead.

As a good practice, it is always recommended to create separate `tasks.py` files in each app. For example, our blog app should have a `blog/tasks.py` file that contains all the Celery tasks associated with the blog app. Let us consider one use case – whenever a blog gets published, we want to send an email to all the blog’s followers. This is an ideal use case where we should let the Celery worker handle the task of sending emails to all the followers. We need to create a `blog/tasks.py` file with the following code:

```
from blog import private
from config.celery import task
@task(bind=True)
def send_email_to_followers(self, author_id, blog_id):
    print('Sending email to all follower logic')
```

In this snippet, we are creating a Celery task called `send_email_to_followers` that sends emails to all the followers of an author whenever they write a new blog.

Now we integrate this Celery task into the Django view. We are calling the Celery task created using the `send_email_to_followers` function and sending it the `author_id` and `blog_id` values as needed:

```
From blog import tasks
def publish_blog(request):
    blog = save_blog(request)
    # publish blog code
    tasks.send_email_to_followers.delay(blog.author_id, blog.id)
```

Please note we are using the `.delay` keyword to call the Celery task. If you don’t use `.delay`, then `publish_blog` will behave like a simple Python function and not be executed by the Celery worker.

We have learned how we can create Celery tasks and use them in Django. In our example, we have used the most basic configuration and task definition possible. As your code grows in size and complexity, you may need to integrate more advanced features of Celery.

### Important note

Celery itself is a framework and has a lot of built-in features. We are only focusing on the high-level interface and important points that one should be aware of as an introduction. It is highly recommended that you go through the official documentation and learn about all the advanced features Celery provides. You can find the official documentation at [https://docs.celeryq.dev/en/stable/userguide/index.html](https://docs.celeryq.dev/en/stable/userguide/index.html)

The advanced features of Celery are out of scope of this book. However, we shall next review a few best practices and advanced concepts that are useful to run Celery and Django in production.

## Best practices for using Celery

Let us learn a few best practices that can help us write Celery tasks in production that is scalable. (as stated, we will focus on introducing the concepts and not diving too deep into the details):

* Using `.delay` to call tasks is a good way to get started with Celery. But as we start moving up to the more advanced concepts of Celery, we should start using `.apply_async` to call Celery tasks. Since `.delay` is an additional wrapper over `.apply_async`, a lot of options are abstracted out that can be helpful.
* Use the `autoretry_for` option in the task definition. This is particularly helpful to let Celery retry whenever it encounters any failure.
* Use the `max_retries` option in the task definition to configure the maximum number of retries Celery can perform before marking the task as failed.
* Always use `time_limit` on long-running tasks. This is a hard limit on tasks. After this threshold is crossed, Celery will kill the task automatically. One should implement a hard limit on all tasks so that long-running tasks don’t use all the workers and choke the task queue.
* Always have short tasks – split long tasks into smaller chunks of tasks. For example, if you have to send emails to 100 users, create a task that would again spawn multiple tasks to send the individual emails to each user.
* Creating Celery tasks that guarantee idempotency and atomic behavior is important to create a scalable Celery system.
* Always make sure to create backward-compatible Celery tasks. For example, if a Celery task takes three arguments in the task definition, such as `def add_task(a,b,c)`, and now we want to remove the third argument, then all the existing tasks created with the three arguments will start failing. Hence, we should make sure to write backward-compatible code so that existing tasks won’t break with future changes.
* As your project grows, you might start using Celery heavily. Celery uses a queue data structure to store the tasks and executes them using `first in, first out` (`FIFO`) order, but there might be use cases where you need to prioritize certain tasks over others. This can be performed by using a different queue, and also by setting the priority while creating tasks. Please read the official documentation on priority for further details at [https://docs.celeryq.dev/en/stable/userguide/routing.html#redis-message-priorities](https://docs.celeryq.dev/en/stable/userguide/routing.html#redis-message-priorities)
* Django signals are synchronous in nature, but by using Celery we can create asynchronous signals.
* Do not pass database objects to tasks, since a database object can be updated by another process. To avoid working on stale data, always pass the object ID to the task. This ensures that the task always gets the latest data to perform operations on.
* Celery assigns task names by default using the module and task name. While this works perfectly when you have good naming conventions, it can cause issues in debugging if the task names are not added properly. Celery has a `name` interface to let you provide custom names for Celery tasks if required.
* If you have important business logic dependent on Celery, always have reconciliation jobs that run periodically to make sure Celery tasks have executed as expected.

We have now learned how to integrate Celery into Django projects and the best practices to follow while implementing Celery in production. There is one more important use case that Celery helps developers tackle – periodic tasks. `celery beat` is a scheduler that checks at regular intervals for any periodic task and creates Celery tasks for the workers to execute.

# Using celery beat with Django
