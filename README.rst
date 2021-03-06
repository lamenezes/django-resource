===============
django-resource
===============

|PyPI latest| |CI Status|

Resource centered REST API clients

-------------------
Ideas on how to use
-------------------

Not all features stated in the following examples are implemented.
This section serves only as motivation for future functionalities.


.. code:: python

    import resources


    class PersonResource(resources.Resource):
        class Meta:
            base_endpoint = 'http://api.com/v1/persons/'


    class PageResource(resources.Resource):
        class Meta:
            endpoints = {
                'delete': 'http://api.com/v1/pages/{}/'
                'filter': 'http://api.com/v1/pages/'
                'get': 'http://api.com/v1/pages/{}/'
                'patch': 'http://api.com/v1/pages/{}/'
                'post': 'http://api.com/v1/pages/'
                'put': 'http://api.com/v1/pages/'
            }

        owner = resources.RelatedField(
            PersonResource,
            source_field='owner_url',  # default is owner_id
            auto_follow=True,  # default is False
        )

        comments = resources.MultipleRelatedField(
            CommentResource,
            source_field='comments_url',
        )


    # GET / single
    person = Person.objects.get(pk=1)
    print(person.name)

    # GET / list
    person_reqset = Person.objects.filter(age=18)
    for person in person_reqset:  # lazy request
        print(person.name)

    # POST
    person = Person.objects.create(name='John Doe', age=18)

    # PATCH
    person.age = 20
    person.save()

    # PUT
    person = Person.objects.update_or_create(name='John Doe', age=30)


.. |PyPI latest| image:: https://img.shields.io/pypi/v/django-resource.svg?maxAge=2592000
    :target: https://github.com/lamenezes/django-resource

.. |CI Status| image:: https://travis-ci.org/lamenezes/django-resource.svg?branch=master
    :target: https://travis-ci.org/lamenezes/django-resource
