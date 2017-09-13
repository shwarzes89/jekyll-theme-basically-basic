---
layout: post
title: Deploing a pure python with Elastic beans talk
description: "Sample post with a background image CSS override."
tags: [aws, python]
---

Elastic beans talk is a suitable service for distributing a application or docker container.
In our teams project, there is a some use case running a task periodically. At first, we choose AWS lambda and Cloudwatch to execute these use case but AWS lambda's timeout limitation is fixed in 30 seconds.  

So, we choose Elastic beans talk for alternatives. It can handle more time for running a task like 500 seconds.

Here be a tutorial post for deploying pure python application to Elastic beans talk.

## 1. Creating a Elastic beans talk application

<figure class="half">
	<a href="/images/aws-ebt/1.png">
	<img src="/images/aws-ebt/1.png" alt=""></a>
</figure>

## 2. Creating a environment

<figure class="half">
	<a href="/images/aws-ebt/2.png">
	<img src="/images/aws-ebt/2.png" alt=""></a>
</figure>


<figure class="half">
	<a href="/images/aws-ebt/3.png">
	<img src="/images/aws-ebt/3.png" alt=""></a>
</figure>
Choose a **'worker tier'**

## 3. Set a base configuration

<figure class="half">
	<a href="/images/aws-ebt/4.png">
	<img src="/images/aws-ebt/4.png" alt=""></a>
</figure>
Choose a platform. In this case, i'm using pure python(Not a Django, Flask).

## 4. Writing a script for deployment

For deploying a application, it needs a kind of option script. First, you shuold make a directory `.ebextensions` into python project root directory then write a option script named `options.config`. This configuration file is for designating where the python file is and set a number of application's processes and threads.

* options.config

```yaml
option_settings:
    aws:elasticbeanstalk:container:python:
        WSGIPath: application.py
        NumProcesses: 3
        NumThreads: 20
```

## 5. Scheduling a task

To run a task periodically, write a crontab script in `cron.yaml` file. If you confused with setting a time, [this site will be helpful](https://crontab.guru/)

* cron.yaml

```yaml
version: 1
cron:
  - name: "someTaskRun"
    url: "/someTask"
    schedule: "0 18 * * *"
```

## 6. Scheduling a task

Wrapping a task file with python application. It will be executed by WSGI with url.
This code reference the sample code of Elastic beans talk.

```python
import logging
import logging.handlers

from wsgiref.simple_server import make_server

# Create logger
logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)

def application(environ, start_response):
    path = environ['PATH_INFO']
    method = environ['REQUEST_METHOD']

    logger.info('start ' + path)
    response = ''

    if method == 'POST':
        try:
            if path == '/someTask':
							# do some task
							pass

            response = 'succefully' + path + ' run '

        except (TypeError, ValueError):
            logger.warning('Error retrieving request body for async work.')

    status = '200 OK'
    headers = [('Content-type', 'text/html')]

    start_response(status, headers)
    return [response]


if __name__ == '__main__':
    httpd = make_server('', 8000, application)
    print("Serving on port 8000...")
    httpd.serve_forever()

```
