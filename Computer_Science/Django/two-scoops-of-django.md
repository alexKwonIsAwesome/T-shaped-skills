# Some facts I didn't know on Django

While reviewing all the django framework's usage, there were many facts that I didn't realize before. This file is to keep in note some points, terms, concepts I learned from the 'Two Scoops of Django' book. It maybe not organized well but at least I can review somethings I newly learned from the book.

## django-braces([https://github.com/brack3t/django-braces](https://github.com/brack3t/django-braces))

This is awesome django package that ables to use clear mixins in the General CBVS.

## Views are simply request and response function

Django Views were mere function that takes HTTP request and turns into HTTP response. This is the basic concept of django views.

## Mixin order in CBV

When using mixins in class based views, there are certain guideline in ordering when using mixins in class inheritence. This rule of order is called `method resolution order` of python. The order proceeds from left to right.

1. The base view classes by Django always go to the right
2. Mixins go to the left of the base view
3. Mixins should inherit from Python's built-in object type
```
# Example
from django.views.generic import TemplateView

class FreshFruitMixin(object):
    
  def get_context_data(self, **kwargs):
    context = super(FreshFruitMixin, self).get_context_data(**kwargs)
    context["has_fresh_fruit"] = True
    return context


class FruityFlavorView(FreshFruitMixin, TemplateView):
  template_name = "fruity_flavor.html"
```
