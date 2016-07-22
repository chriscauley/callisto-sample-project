Callisto Sample Project
==============================

This project demonstrates example usage of callisto-core. The hope is to encourage forks of callisto-core for domains other than U.S. college campuses.

.. image:: https://img.shields.io/badge/built%20with-Cookiecutter%20Django-ff69b4.svg
     :target: https://github.com/pydanny/cookiecutter-django/
     :alt: Built with Cookiecutter Django

.. image:: https://codeclimate.com/github/SexualHealthInnovations/callisto-sample-project/badges/gpa.svg
   :target: https://codeclimate.com/github/SexualHealthInnovations/callisto-sample-project
   :alt: Code Climate

LICENSE: GPLv3

Code of conduct
-----------------

This project adheres to a code of conduct(https://github.com/SexualHealthInnovations/callisto-core/blob/master/CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to tech@sexualhealthinnovations.org.


Setting up a development environment
------------

Install pip, virtualenv, and npm if you don't have them already.

Clone this repo.

Make a virtual environment::

    $ mkvirtualenv --python=python3 callisto-sample-project

Install npm depenencies::

  $ npm install

Install pip dependencies::

  $ pip install -r requirements/local.txt

Make Callisto importable (may need to change the path to your virtual environment)::

  $ touch ~/.virtualenvs/callisto-sample-project/lib/python2.7/site-packages/callisto/__init__.py

Create a database::

  $ createdb callisto-sample-project

Run the migrations::

  $ ./manage.py migrate

Run your first development server::

  $ npm start



Settings
------------

Moved to settings_.

.. _settings: http://cookiecutter-django.readthedocs.io/en/latest/settings.html

Basic Commands
--------------

Setting Up Your Users
^^^^^^^^^^^^^^^^^^^^^

* To create a **normal user account**, just go to Sign Up and fill out the form. Once you submit it, you'll see a "Verify Your E-mail Address" page. Go to your console to see a simulated email verification message. Copy the link into your browser. Now the user's email should be verified and ready to go.

* To create an **superuser account**, use this command::

    $ python manage.py createsuperuser

For convenience, you can keep your normal user logged in on Chrome and your superuser logged in on Firefox (or similar), so that you can see how the site behaves for both kinds of users.


Configuring a survey
^^^^^^^^^^^^^^^^^^^^^

* Initialize the wizard builder with an empty page.

    $ python manage.py shell
    >>> from wizard_builder.models import QuestionPage
    >>> QuestionPage.objects.create()

* Log into http://localhost:8000/admin/ with your Django superuser name and password.

* Go to http://localhost:8000/admin/wizard_builder/ and add a Form page.

* Go to http://localhost:8000/reports/new/1, answer the question, and click submit.

* TO DO: have this work :)

Test coverage
^^^^^^^^^^^^^

To run the tests, check your test coverage, and generate an HTML coverage report::

    $ coverage run manage.py test
    $ coverage html
    $ open htmlcov/index.html

Running tests with py.test
~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ py.test


Running javascript tests with karma
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ npm test


Hot reloading with React and Webpack
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install npm depenencies::

  $ npm install

Start the development server::

  $ npm start


A more detailed explanation for `Developing locally with webpack`_


The `static project readme`_ contains a lot of information about React / Redux and Webpack for this project.

.. _`static project readme`: callisto-sample-project/static/callisto-sample-project/README.md
.. _`Developing locally with webpack`: http://cookiecutter-django.readthedocs.io/en/latest/developing-locally-webpack.html







Email Server
^^^^^^^^^^^^

In development, it is often nice to be able to see emails that are being sent from your application. For that reason local SMTP server `MailHog`_ with a web interface is available as docker container.

.. _mailhog: https://github.com/mailhog/MailHog

Container mailhog will start automatically when you will run all docker containers.
Please check `cookiecutter-django Docker documentation`_ for more details how to start all containers.

With MailHog running, to view messages that are sent by your application, open your browser and go to ``http://127.0.0.1:8025``





Sentry
^^^^^^

Sentry is an error logging aggregator service. You can sign up for a free account at  https://getsentry.com/signup/?code=cookiecutter  or download and host it yourself.
The system is setup with reasonable defaults, including 404 logging and integration with the WSGI application.

You must set the DSN url in production.




Deployment
----------



Heroku
^^^^^^

.. image:: https://www.herokucdn.com/deploy/button.png
    :target: https://heroku.com/deploy

See detailed `cookiecutter-django Heroku documentation`_.

.. _`cookiecutter-django Heroku documentation`: http://cookiecutter-django.readthedocs.io/en/latest/deployment-on-heroku.html





Docker
^^^^^^

See detailed `cookiecutter-django Docker documentation`_.

.. _`cookiecutter-django Docker documentation`: http://cookiecutter-django.readthedocs.io/en/latest/deployment-with-docker.html

Quick `docker-compose` guide (for development):

* Install `Docker`_ and `docker-compose`_. Note: the `Mac version of Docker`_ comes with ``docker-compose`` included. 

* In one terminal, at the root of the project, run: ``docker-compose -f dev.yml up --build postgres``. It will take
  some time for postgres to install the first time you run the command. The last line of output you will probably see
  will be something like ``postgres_1  | LOG:  autovacuum launcher started``. That indicates that postgres is ready and
  waiting. 

* In another terminal, also at the root of the project, run: ``docker-compose -f dev.yml up --build mailhog npm
  django``. You may have to wait quite some time before webpack finishes building the bundle of static files. If you
  try to access the site before it's finished, you'll probably see a traceback for ``builtins.OSError
: Error reading /app/webpack-stats.json. Are you sure webpack has generated the file and the path is correct?``. Once
you see ``npm_1       | webpack: bundle is now VALID.``, your django/npm/mailhog server is running.

* If you want to run any ``manage.py`` command (like ``migrate`` or ``shell_plus``), ensure the `django` container is up, and then run

::

  docker-compose -f dev.yml run django python /app/manage.py shell_plus

Note: You may need to prepend those commands with `sudo`.

.. _`Docker`: https://docs.docker.com/engine/installation/
.. _`docker-compose`: https://docs.docker.com/compose/install/
.. _`Mac version of Docker`: https://docs.docker.com/docker-for-mac/
