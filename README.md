<h1 align="center">ft_transcendence</h1>

<p align="right">Final score: 125</p>

ft_trancsendence is the last group project of 42 common core. It is about developing a full-stack website within a docker environment. The subject holds every constraint.

[Aanghi](https://github.com/AnghiAndrei), having worked for two years as a web-developer amongst other projects and experiences, did all of the frontend and helped the rest of the team (me, [ecoli](https://github.com/edoardoColi) and fcardina) with the backend development.

This web-application is a SPA that uses REST APIs to fetch data from the backend, the final result
being a site where users can register, play "Pong" together and more. What defines an SPA amongst
other features is routing happens entirely on client-side, so the web-application looks like any
desktop application. This means one can click on a link on the webpage and be redirected without having to
refresh the page, also the "back and forward" web navigation arrows are enabled inside the web browser.

Javascript (the Bootstrap framework) is used for the frontend. It can dynamically generate "divs"
and load them into the current DOM of the user's browser, enabling a smooth navigation. The CSS-Bootstrap
library is used to create good looking user interfaces, and the index.html acts as backbone for the frontend.

Python (the Django REST framework) is used for the backend. To create a REST API with Django you need to define
a model (the type of data), an URL (the location where the request will be made at) and a view (a function
that defines what happens when an URL is reached). In very few words, a REST API is one of two endpoints inside an
exchange of data. Django also comes with a number of ways to sanitize data from users, can prevent against SQL injections and XSS attacks.

https://docs.djangoproject.com/en/5.1/intro/overview/

https://docs.djangoproject.com/en/5.1/topics/security/

https://www.sharpcoderblog.com/blog/mastering-django-forms-for-user-input-and-validation

Postgresql is used to enable data persistency. The DB is just one, but every Django docker is implemented
as a micro-service: they all fetch data from postgres and if one of them stops unexpectedly the others are
not affected. This will result in the specific service being unreachable from the frontend.

Last but not least this website is accessible only through NGINX running on port 8000. That is the only port
that is exposed. NGINX and Django have been configured in a way that Django containers are made not accessible from outside our frontend, also
the reverse proxy is configured to limit the request rate of REST APIs to 10 requests per second:

limit_req_zone $binary_remote_addr zone=users_limit:10m rate=10r/s;

limit_req_zone $binary_remote_addr zone=gama1_limit:10m rate=10r/s;

NGINX is also used to enable the Json Web Token to be held inside the header (under "Authorization") of any request to the backend:

add_header Access-Control-Allow-Headers "Content-Type, Authorization";

this adds a layer of authenticity to the source's request for inside every Django view a decode of the JWT takes place and
if the decode is unsuccesful the request is denied. The tokens are issued during registration and are unique
for every online user. 2FA is also implemented during user registration. 

Every module we've done is written in ./frontend/app/module.txt and described in the subject.

To visit this website through a web browser, first you should clean your docker environment by running these
commands from the root of this project directory:

$docker image prune --all --force

$docker system prune --all --force --volumes

Then:

$docker-compose up -d --build

After the execution of this command "https://localhost:8000" will be reachable from a web browser.
