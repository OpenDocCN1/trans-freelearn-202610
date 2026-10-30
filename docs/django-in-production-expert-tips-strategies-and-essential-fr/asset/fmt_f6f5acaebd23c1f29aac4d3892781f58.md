
*   The difference between synchronous and asynchronous programming, and the advantages of the latter over the former.
*   The basic concepts of messaging brokers and task queues. `Celery` is a framework used to process task queues and task scheduling.
*   How to set up Redis and use a remote Redis server in local system (we discussed this in [*Chapter 6*](#B18867_06.xhtml#_idTextAnchor145)).

# Asynchronous programming in Django

Django 4 introduced the concept of asynchronous programming in its core framework. While support for async views is introduced in Django, the official documentation ([https://docs.djangoproject.com/en/stable/topics/async/#performance](https://docs.djangoproject.com/en/stable/topics/async/#performance)) states that async views would lead to a performance hit.

Hence, the adaption of native asynchronous programming is not yet mature as of Django 4.x. While most common and basic features will not need any async support, Django as a framework is thus lagging behind other frameworks such as Express and Spring. To work around this problem, Django developers use third-party frameworks to implement asynchronous support. Generally, asynchronous programming is helpful when one has a long-running task that can be executed in the background. For example, when we have to send an email, our Django view would make an external API call to an email service and await the response. This whole process can take a while and there is no added value in making client requests wait for the email service; rather, it is always preferred to perform this task in the background. For such async long-running/background tasks, one can use frameworks such as `Celery`. `Celery` is a distributed task queue to offload tasks from the main request-response workflow.

Note

One should remember that `Celery` itself is a framework and has its own capabilities and a complex architecture. The architecture of `Celery` is beyond the scope of this book. We shall focus only on the integration of `Celery` with Django in this chapter. To gain more knowledge of the architecture and advanced concepts of `Celery`, you are advised to go through the `Celery` official documentation at [https://docs.celeryq.dev/en/stable/index.html](https://docs.celeryq.dev/en/stable/index.html).

In the next section, we will learn how to use `Celery` with Django and leverage the capabilities of `Celery` to perform long-running tasks in the background.

# Using Celery with Django

Let us try to understand how `Celery` works under the hood by taking a small real-world example. Imagine you want to perform 10 different tasks and you have 2 people to work on them. Each person can work on only one particular task at a time, and new tasks might arrive at any given time. To solve this problem, the workers would create a list, and every time a new task comes in, they would add the task to that list. Each time a person finishes their task, you would strike off the task from the list and then assign them the next task from the top. If you add more people to work for you, then you can finish your tasks faster. This is what `Celery` does.

In `Celery`, there is a task queue that keeps track of all the tasks coming in – task queues are saved using RabbitMQ or Redis. There are `Celery` workers that pick tasks from the queue and work on them. Django generates these tasks and pushes them to the task queue, from where `Celery` picks tasks and executes them using `Celery` workers.

Let us now see how we can integrate `Celery` into a Django project. We shall use Redis to create and maintain the `Celery` task queue in our project. In *Figure 8**.1*, we have a minimalistic `Celery`-Django architecture:

![Figure 8.1: Celery architecture with Django](image/Figure_8.1_B18867.jpg) 

Figure 8.1: Celery architecture with Django

The application servers running in Django create tasks and send those tasks to the Redis task queue. Then the `Celery` workers pick these tasks one by one from the queue and run them. This is an overly simplified example of how `Celery` and Django work under the hood, but it should give you a good idea about the architecture.

Let us now see how we can install `Celery` and integrate it into our system.

## Integrating Celery and Django

`Celery` provides multiple broker integrations such as RabbitMQ, Redis, and Amazon SQS. We shall use Redis as a broker for our `Celery` service. In [*Chapter 6*](#B18867_06.xhtml#_idTextAnchor145), we learned how to use a remote Redis server locally, and we will use the same Redis server here for our `Celery` broker. Let us look into the steps to integrate `Celery` into Django:

1. As a first step, we need to install `Celery` in our project using the `pip install celery` command.

2. Create a new `celery.py` file in the same folder where we have the `settings.py` file containing the following code (for us, the location is `backend/config/celery.py`):

```
import os
from celery import Celery
celery_settings_value = "<project name>.settings"
# change <project name> with folder name where your settings.py file is present.
os.environ.setdefault("DJANGO_SETTINGS_MODULE", celery_settings_value)
app = Celery("<project name>")
# change <project name> with folder name where your settings.py file is present.
app.config_from_object("django.conf:settings", namespace="CELERY")
app.autodiscover_tasks()
task = app.task
@app.task(bind=True)
def debug_task(self, data):
    print(data)
```

3. Now we have to add the Redis broker connection details to the `settings.py` file. You need to update the Redis connection details in RedisLabs. In this step, we are telling `Celery` to use Redis as a broker and result backend:

```
REDIS_CONNECTION_STRING = '<user>:<pass>@<endpoint>'
# Use redis lab credentials
CELERY_BROKER_URL = f"redis://{REDIS_CONNECTION_STRING}"
CELERY_RESULT_BACKEND = f"redis://{REDIS_CONNECTION_STRING}"
```

We have integrated Django, `Celery`, and Redis. `Celery` uses workers to execute tasks. Now we need to define the workers. Just like the Django management command, `Celery` also has a management command that can be used to define the workers.

4. Open a new terminal and run the following command (please note – the `<project>` keyword needs to be replaced with the folder name where your `celery.py` file is located):

```
celery --app=<project> worker --loglevel=INFO
```

This command creates a `Celery` worker that can be used to test and work in the local development environment. If our integration is successful, we should be able to see output shown in *Figure 8**.2*:

![Figure 8.2: Terminal running Celery workers](image/Figure_8.2_B18867.jpg) 

Figure 8.2: Terminal running Celery workers

Once we have confirmed the `Celery` worker is running, we can stop the worker process and move to the Django integration step.

1. To test our integration, let us call our `Celery` task from a Django view. Add the following code to the `blog/views.py` file:

```
from config.celery import debug_task
@api_view(['GET'])
def verify_blog(request):
    verify_word = request.GET.get('verify_word')
    debug_task.delay(f"Celery Task verification: {verify_word}")
    return Response({'status': 'success'})
```

*Please note that we are importing `debug_task` from `config.celery` because we have created the `celery.py` file inside the `config` folder/app. Your version might have any other name, so make sure to check.*

Now we need to link our view to the URLs. Add the following code to the `blog/urls.py` file:

```
urlpatterns = [
    path('verify/', views.verify_blog)
]
```

We have successfully integrated `Celery` into our Django project.

Now to verify whether our `Celery` integration is working as expected, let us run the `Celery` worker on a terminal by running the following command:

```
celery --app=config worker --loglevel=INFO
```
