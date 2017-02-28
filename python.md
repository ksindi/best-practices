> A universal convention supplies all of maintainability, clarity, consistency, and a foundation for 
> good programming habits too. What it doesn't do is insist that you follow it against your will.
> That's Python! [Tim Peters on comp.lang.python, 2001-06-16]

## Style Guide

Use Google's Python Style Guide [[1](http://sphinxcontrib-napoleon.readthedocs.org/en/latest/example_google.html)
and [2](https://google-styleguide.googlecode.com/svn/trunk/pyguide.html)].
Documentation can be automatically generated with
[Napoloen](http://sphinxcontrib-napoleon.readthedocs.org/en/latest/index.html).

## Project Structure

See my [cookiecutter pypackage](https://github.com/ksindi/cookiecutter-pypackage).

## Linting

Use [flake8](https://flake8.readthedocs.io/en/latest/).

Python style error codes: https://pycodestyle.readthedocs.io/en/latest/intro.html#error-codes

## Unit testing

Use [doctest](https://docs.python.org/3/library/doctest.html) for small utility functions that never change.

Use [pytest](http://doc.pytest.org/en/latest/) for larger unittests.

Other tools: [mock](https://pypi.python.org/pypi/mock)

## Branch Naming

Follow the convention: `{ticket}-{description}`.

Example: `ticket-123-some-description`

## Commits

Commit messages should be in the form: `{prefix} [{ticket}] {descriptive message}`.

Example: `ENH [TICKET-123] add foo script`

Label prefixes follow pandas [guidelines](http://pandas.pydata.org/pandas-docs/stable/contributing.html#committing-your-code):

> ENH: Enhancement, new functionality  
> BUG: Bug fix  
> DOC: Additions/updates to documentation  
> TST: Additions/updates to tests  
> BLD: Updates to the build process/scripts  
> PERF: Performance improvement  
> CLN: Code cleanup

## Resources

List of resources that advise on Python best practices, in order of relevance:
- [Effective Python](http://www.amazon.com/Effective-Python-Specific-Software-Development/dp/0134034287) by Brett Slatkin [Book]
- [Refactoring Python](https://speakerdeck.com/pycon2016/brett-slatkin-refactoring-python-why-and-how-to-restructure-your-code) [Slides]
- [Elements of Python Style](https://github.com/amontalenti/elements-of-python-style) [URL]
- [Beyond PEP 8](https://www.youtube.com/watch?v=wf-BqAjZb8M) [Video]
- [Writing Python 2-3 compatible code](http://python-future.org/compatible_idioms.html) [URL]
- [Transforming Code into Beautiful, Idiomatic Python](https://www.youtube.com/watch?v=OSGv2VnC0go) [Video]
- [Python Best Practice Patterns by Vladimir Keleshev](http://stevenloria.com/python-best-practice-patterns-by-vladimir-keleshev-notes/) [URL]
- [Clean Code](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882/): In Java but still useful [Book]

## Examples of Well Written Python Code

Code repositories that make good use of the language:

- [Requests](https://github.com/kennethreitz/requests)
- [joblib](https://github.com/joblib/joblib)
- [MoviePy](https://github.com/Zulko/moviepy)
