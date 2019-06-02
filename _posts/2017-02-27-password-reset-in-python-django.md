---
layout: post
title:  "Password Reset in Django"
date:   2017-02-27 22:22:32 +0530
permalink: /password-reset-in-django/
---
This tutorial will build on the previous one, and will deal with the issue of password reset through email in Django. All the code builds upon that of the previous tutorial and the code for the present post can be found [here](https://github.com/khansubhan95/Django-Password-Reset) . We will use Python 2.7 and Django 1.9. Let’s get started.

I will assume that you have the code from the previous tutorial, if not you can find it [here](https://github.com/khansubhan95/Django-User-Auth) . Note that I have replaced the src/settings.py file with src/settings.py.template to avoid disclosing passwords. The lines of code to be added in existing files will be represented in bold.

As mentioned in the previous post, Django has several [views](https://docs.djangoproject.com/en/1.10/topics/auth/default/#using-the-views) for user authentication. Amongst these are views for password reset. There are 4 views that will be of interest in password reset and I will try to detail them here.

Before sending email, you will need to set up the src/settings.py to do so. Include the following variables

```
EMAIL_BACKEND = ‘django.core.mail.backends.smtp.EmailBackend’
EMAIL_HOST = ‘’ # mail service smtp
EMAIL_HOST_USER = ‘’ # email id
EMAIL_HOST_PASSWORD = ‘' #password
EMAIL_PORT = 587
EMAIL_USE_TLS = True
```

Normally, in a large website you will use a third party mail service like Sendgrid for sending password reset emails. However for the purposes of this tutorial, a Gmail account will suffice. The `EMAIL_HOST` for Gmail is ‘smtp.gmail.com’. If you want, you may also enclose sensitive information into a json file and import it into src/settings.py.

### The password_reset view
To begin using this view, it is essential that you add the following view before using any password reset related views to src/urls.py

```
urlpatterns = [
    url(r’^admin/‘, admin.site.urls),
    url(r’^login/$’, auth_views.login),
    url(r’^logout/$’, auth_views.logout),
    url(r’^’, include(‘mysite.urls’)),
    **url(‘^’, include(‘django.contrib.auth.urls’)),**
]
```

Note that when I say before, it means absolutely before. This view should precede all the password reset views. The reason to include this view will become clear in a moment

To use the [password_reset](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.views.password_reset) view, you will need to include the following `urlpattern`

```
urlpatterns = [
    url(r’^admin/‘, admin.site.urls),
    url(r’^login/$’, auth_views.login),
    url(r’^logout/$’, auth_views.logout),
    url(r’^’, include(‘mysite.urls’)),
    url(‘^’, include(‘django.contrib.auth.urls’)),
    url(r’^password_reset/$’, auth_views.password_reset),
}
```

The view presents a form in registration/password_reset_form.html to get the user’s email address(so that the email can be mailed to them). However if no such email address exists for a user, then no email is sent, as well as no error is shown by the user. Here is a sample registration/password_reset_form.html file

```
{% raw %}{% load i18n %}{% endraw %}
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <form method=“post”>{% raw %}{% csrf_token %}{% endraw %}
        <div>
            {{ form.email.errors }}
            <label for=“id_email”>{% raw %}{% trans ‘Email address:’ %}{% endraw %}</label>
            {{ form.email }}
        </div>
        <input type=“submit” value=“{% raw %}{% trans ‘Reset my password’ %}{% endraw %}” />
    </form>
</body>
</html>
```

If you don’t want to use the default registration/password_reset.html template, you can include a third parameter in the `urlpattern` which is a Python dictionary like so

```
url(r’^password_reset/$’, auth_views.password_reset, {‘template_name’: ‘accounts/reset_password.html’}),
```

Pretty much the only thing that you need to pay attention to is `{{ form.email }}`. This renders an html input of type email, and sets the name of the input attribute to email, which is used to refer to the user’s email. It also adds other attributes. It is rendered as follows

```
<input id=“id_email” maxlength=“254” name=“email” type=“email” />
```

You might be wondering what the form variable is, if we have not passed anything with that name to the view. The password_reset view by default will pass a [PasswordResetForm](https://docs.djangoproject.com/en/1.8/topics/auth/default/#django.contrib.auth.forms.PasswordResetForm) as the form to get the user’s email. So no need to create another form to get the user’s email. `PasswordResetForm` is the reason why we included the `urlpattern` mentioned at the beginning of this tutorial. This form requires that particular urlconf.

We also need to specify a template for the email to be sent. By default this template is the registration/password_reset_email.html. Here is a sample that is from the Django documentation

```
Someone asked for password reset for email {{ email }}. Follow the link below:
{{ protocol}}://{{ domain }}{% raw %}{% url ‘password_reset_confirm’ uidb64=uid token=token %}{% endraw %}
```

The part we are interested in is

```
{{ protocol}}://{{ domain }}{% raw %}{% url ‘password_reset_confirm’ uidb64=uid token=token %}{% endraw %}
```

The [documentation](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.views.password_reset) also described what the variables entered in the template are as well as additional variables that the template has access to. The `{{ email }}` represent the user’s email (if such an email existed), `{{ domain }}` the web application’s domain, `{{ protocol }}` will be either http or https. The `{% raw %}{% url %}{% endraw %}` is a [template tag](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#url) which returns the absolute path without the domain name. Don’t worry about what `passsword_reset_confirm` (url corresponding to password_reset_confirm view, which is described below) is now, `uid` represents the users primary key in base64 and `token` will be used to authenticate the user. Basically the url will be as follows, if you are using the development server.

```
http://localhost:8000/reset/xxx/xxxxxxxx
```

The user will obtain the password reset form at the above url.

### The password_reset_done view
After the user has entered the email id and submitted, and upon sending an email, we want to inform the user that a mail has been sent successfully. This can be done using the [password_reset_done](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.views.password_reset_done) view. To obtain this append the following urlpattern in src/urls.py

```
urlpatterns = [
    url(r’^admin/‘, admin.site.urls),
    url(r’^login/$’, auth_views.login),
    url(r’^logout/$’, auth_views.logout),
    url(r’^’, include(‘mysite.urls’)),
    url(‘^’, include(‘django.contrib.auth.urls’)),
    url(r’^password_reset/$’, auth_views.password_reset),
    url(r’^password_reset/done/$’, auth_views.password_reset_done),
]
```

The view by default displays the template registration/password_reset_done.html

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    A password reset email has been sent to the entered email, if a user with such an email address exists
</body>
</html>
```

Now run the dev server (`python manage.py runserver`) and check the above two features by navigating to /password_reset. Make sure that you already have a user account and the account is made with a valid email id, which you have access to.

### The password_reset_confirm view.

Remember at the end of password_reset, I showed a url where the user will get the password reset form?

```
{% raw %}{% url ‘password_reset_confirm’ uidb64=uid token=token %}{% endraw %}
```

At that point I had not described what the `password_reset_confirm` was. It represents the url corresponding to the [password_reset_confirm](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.views.password_reset_done) view, and the view is included as follows

```
urlpatterns = [
    url(r’^admin/‘, admin.site.urls),
    url(r’^login/$’, auth_views.login),
    url(r’^logout/$’, auth_views.logout),
    url(r’^’, include(‘mysite.urls’)),
    url(‘^’, include(‘django.contrib.auth.urls’)),
    url(r’^password_reset/$’, auth_views.password_reset),
    url(r’^password_reset/done/$’, auth_views.password_reset_done),
    url(r’^reset/(?P<uidb64>[0-9A-Za-z_\-]+)/(?P<token>[0-9A-Za-z]{1,13}-[0-9A-Za-z]{1,20})/$’,
    auth_views.password_reset_confirm),
]
```

We are setting the url /reset to render the password_reset_confirm view. The `password_reset_confirm` in the url template tag corresponds to exactly this i.e. /reset and thus the url

```
{{ protocol}}://{{ domain }}{% raw %}{% url ‘password_reset_confirm’ uidb64=uid token=token %}{% endraw %}
```

corresponds to this

```
https://localhost:8000/reset/xxx/xxxxx
```

This view will render the template registration/password_reset_confirm.html

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <form method=“POST”>
        {% raw %}{% csrf_token %}{% endraw %} {{ form.as_p }}
        <button type=“submit”>Submit</button>
    </form>
</body>
</html>
```

Again the part of interest here is form. As in password_reset view, the password_reset_confirm view has a default form, [SetPasswordForm](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.forms.SetPasswordForm) , and this gets set to the value `form`.

### The password_reset_complete view
After the user has entered a new password, we want to display a template saying that it has been done so. We can use the [password_reset_complete](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.views.password_reset_confirm) view for this purpose. Append the following urlpattern to src/settings.py

```
urlpatterns = [
    url(r’^admin/‘, admin.site.urls),
    url(r’^login/$’, auth_views.login),
    url(r’^logout/$’, auth_views.logout),
    url(r’^’, include(‘mysite.urls’)),
    url(‘^’, include(‘django.contrib.auth.urls’)),
    url(r’^password_reset/$’, auth_views.password_reset),
    url(r’^password_reset/done/$’, auth_views.password_reset_done),
    url(r’^reset/(?P<uidb64>[0-9A-Za-z_\-]+)/(?P<token>[0-9A-Za-z]{1,13}-[0-9A-Za-z]{1,20})/$’,
    auth_views.password_reset_confirm),
    url(r’^reset/done/$’, auth_views.password_reset_complete),
]
```

The view renders a template which by default is located in registration/password_reset_complete.html

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    Password was successfully reset.
    <a href=“/login”>Login</a>
</body>
</html>
```

If everything went alright, you would have received an email and upon going to the link given in it, able to reset the password and see the successfully changed password page.

Note that in this post I did not mention some things for the sake of brevity. For instance, how to use templates if you don’t want the location to be registration/, as well as other arguments for the views that are mentioned in the documentation (this can be done by giving a third parameter to the view, which is a Python dictionary, with the keys as the names of the argument, and the values as the values of the argument, here is a typical such key value pair `{‘template_name’: ‘accounts/reset_password.html’}`.

Here’s some relevant documentation in case you get stuck, or want to learn more:
* [User authentication views](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.forms.SetPasswordForm) 
* [Source code](https://docs.djangoproject.com/en/1.8/_modules/django/contrib/auth/forms/) of PasswordResetForm and SetPasswordForm

You can also read this post at [Medium](https://medium.com/@khansubhan95/password-reset-in-django-8b4d37924958)
