=====================
Django WordPress Auth
=====================

Introduction
============

Allows for access in Django to a WordPress installation for checking for
things like login status.

Requirements
============

WordPress and Django should be on the exactly same domain. But, if you want to use different sub-domains, you might have to modify some WordPress's constants (e.g wp-includes/default-constants.php).

Python Dependencies:

 * `phpserialize`_ (installed automatically with pip)

You need a WordPress plugin in certain cases, to share the session cookie with Django.
It isn't mandatory if WordPress is installed on the root directory of the domain.

 * `root Cookie`_
 
 .. _`phpserialize`: http://pypi.python.org/pypi/phpserialize
 .. _`root Cookie`: http://wordpress.org/extend/plugins/root-cookie/


Alternatives to Root Cookie
---------------------------

If you are receiving warnings using *root Cookie*, please see `this issue`_ for alternative solutions.

 .. _`this issue`: https://github.com/dellis23/django-wordpress-auth/issues/6


Installation
============

.. sourcecode:: bash

    $ pip install django-wordpress-auth-lite

Add your WordPress's auth keys and salts (found in wp-config.php) to your settings.py.

.. sourcecode:: python

    WORDPRESS_LOGGED_IN_KEY = "rs&^D%jPdu=vk|VVDsdfsdgsdgsdg9sd87f98s7h[Xm$3gT/@1xdasd"
    WORDPRESS_LOGGED_IN_SALT = "3]x^n{d8=su23902iu09jdc09asjd09asjd09jasdV-Lv-OydAQ%?~"

Add your WordPress database to DATABASES in settings.py.

.. sourcecode:: python

    DATABASES = {
        'default': {
            ... # default django DB
        },
        'wordpress': {  # must be named 'wordpress'
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'wordpress',
            'USER': '...',
            'PASSWORD': '...',
            'HOST': '...',
            'PORT': 3306,
        }
    }

Add the middleware to MIDDLEWARE_CLASSES in settings.py.
Make sure it's placed somewhere after the session middleware.

.. sourcecode:: python

    MIDDLEWARE_CLASSES = (
        'django.contrib.sessions.middleware.SessionMiddleware',
        # ...
        'wordpress_auth_lite.middleware.WordPressAuthMiddleware',
    )

Finally, add `wordpress_auth_lite` to INSTALLED_APPS.

.. sourcecode:: python

    INSTALLED_APPS = (
        # ...
        'wordpress_auth_lite',
    )

Usage
=====

To restrict a view to users that are authenticated with WordPress, or redirect them to the WordPress's login page otherwise:
``wordpress_login_required`` decorator.

.. sourcecode:: python

    from wordpress_auth_lite.decorators import wordpress_login_required

    @wordpress_login_required
    def my_view():
        pass

Finally, the middleware provides access to the WordPress user via ``request.wordpress_user``.

See ``models.py`` for full reference.
