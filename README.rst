Dvalidator
==================

**Library for validating your object dictionary**

.. code-block:: python

    from dvalidator.validator import Validator
    import dvalidator.rules as rules


    validator = Validator({
        'a': rules.required(),
        'b': rules.required('Field "b" not found'),
        'c': [rules.required(), rules.is_type(str, 'Field "c" is not str type')]
        })

    d = {'a': 1, b: 2, c: '3'}

    res, err = validator.validate(d)

    print(res)
    True

Other rules see in: https://github.com/kakabara/capybara/blob/master/capybara/rules.py


Also you can create custom validation method:

.. code-block:: python

    from dvalidator.validator import Validator
    import dvalidator.rules as rules


    def raise_on_exist(message='Found unexpected field in dict'):
        def check(field: str, dict_to_check: dict):
            if field in dict_to_check:
                return False, None
            else:
                return True, message
        return check

    validator = Validator({
        'a': rules.required(),
        'b': raise_on_exist()
        })

    d = {'a': 1, 'b': 2}

    res, err = validator.validate(d)

    print(res)
    False

