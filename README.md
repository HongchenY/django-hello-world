# Defang x Django

This template is a simple "Hello World" project developed using Django. It is ready to be deployed via Defang. You could download Defang by following the instructions at this link (https://github.com/defang-io/defang).

Once Defang has been properly installed, you could directly launch this template project to the world with a single command.

# How to Deploy it to the WORLD?
1. Go to this link (https://github.com/defang-io/defang) and download Defang by following the instructions.
2. Open terminal and make sure that you are in the correct directory.
3. If you haven't logged in, type `defang login` and click the link provided to login/register.
4. Once you have logged in, type `defang compose up`
5. Click the URL provided; it would typically take a few minutes for the page to reveal.
6. Now, the world could see your work.

# What is Needed for a Successful Deployment?
1. Make sure your code works. If your code fails locally, it will not be successfully deployed.
2. Defang requires a Dockerfile, and a docker-compose file to deploy your code. Make sure your containers are correct.
3. If you are using Defang playground, build a .dockerignore file to comply with the usage limit.
For detailed instructions, please go to (https://docs.defang.io/docs/intro)

# Use Your Own Cloud Accounts to Deploy
Defang's goal is to make it easier to deploy your workloads to your own cloud accounts. We refer to this as bring-your-own-cloud (BYOC).
You could use your own credits from platforms like AWS, Azure, etc to deploy your applications.
For detailed instuctions of BYOC, please go to (https://docs.defang.io/docs/concepts/defang-byoc)


# How this Template Works
Our Django application, `example` is configured as an installed application in `defang_app/settings.py`:

```python
# defang_app/settings.py
INSTALLED_APPS = [
    # ...
    'example',
]
```

The `wsgi` module must use a public variable named `app` to expose the WSGI application:

```python
# vercel_app/wsgi.py
app = get_wsgi_application()
```

The corresponding `WSGI_APPLICATION` setting is configured to use the `app` variable from the `vercel_app.wsgi` module:

```python
# vercel_app/settings.py
WSGI_APPLICATION = 'defang_app.wsgi.app'
```

There is a single view which renders the current time in `example/views.py`:

```python
# example/views.py
from datetime import datetime

from django.http import HttpResponse


def index(request):
    now = datetime.now()
    html = f'''
    <html>
        <body>
            <h1>Defang says: Hello World!</h1>
            <p>The current time is { now }.</p>
        </body>
    </html>
    '''
    return HttpResponse(html)
```

This view is exposed a URL through `example/urls.py`:

```python
# example/urls.py
from django.urls import path

from example.views import index


urlpatterns = [
    path('', index),
]
```

Finally, it's made accessible to the Django server inside `defang_app/urls.py`:

```python
# vercel_app/urls.py
from django.urls import path, include

urlpatterns = [
    ...
    path('', include('example.urls')),
]
```

This example uses the Web Server Gateway Interface (WSGI) with Django to enable handling requests on Vercel with Serverless Functions.

## Running Locally

If you want to run it before deploying it to the internet, use the following command.

```bash
python3 manage.py runserver
```

Your Django application is now available at `http://localhost:8000`.



