# Applying google analytics on Django powered website

## Using django-google-analytics package
reference: [https://pypi.python.org/pypi/django_google_analytics/2](https://pypi.python.org/pypi/django_google_analytics/2)

Follow the instructions of the pypi package.
1. ```pip install django_google_analytics```
2. Ensure you have sites configured. https://docs.djangoproject.com/en/1.8/ref/contrib/sites/#enabling-the-sites-framework
3. add `analytics` to your `INSTALLED_APPS`
4. `python manage.py migrate`
5. In the django admin interface visit the "Sites"
page and add your google web property id for the site you
wish to track.
6. Add the following to your desired templates.
`{% load googleanalytics %}`
7. at the very bottom of your head section add this
`{% analytics %}`
8. if you want to avoid counting logged in user visits
surround the above line with the following
`{% if not user.is_staff %}`
`{% endif %}`
9. Go to your google analytics control panel and refresh
frantically to see if it is working  
