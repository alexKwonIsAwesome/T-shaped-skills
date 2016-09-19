# Applying google analytics on Django powered website

## Using django-google-analytics package
reference: [https://pypi.python.org/pypi/django_google_analytics/2](https://pypi.python.org/pypi/django_google_analytics/2)

Follow the instructions of the pypi package.
- ```pip install django_google_analytics```
- Ensure you have sites configured. https://docs.djangoproject.com/en/1.8/ref/contrib/sites/#enabling-the-sites-framework
- add `analytics` to your `INSTALLED_APPS`
- `python manage.py migrate`
- In the django admin interface visit the "Sites" page and add your google web property id for the site you wish to track.
- Add the following to your desired templates.`{% load googleanalytics %}`
- At the very bottom of your head section add this`{% analytics %}`
- if you want to avoid counting logged in user visits surround the above line with the following.
```
{% if not user.is_staff %}
{% endif %}
```
- Go to your google analytics control panel and refresh frantically to see if it is working
