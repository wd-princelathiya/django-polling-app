This project is a [basic poll application](https://docs.djangoproject.com/en/5.1/intro/tutorial01/) and uses Python and Django with SQlite3.

"The participants will construct software with security flaws, point out the flaws in the project, and provide the steps to fix them."

***NOTE: The project uses a local SQlite database, and SHOULD NOT be used in production environment.***

### Vulnerability 1: Broken Access Control
First flaw is OWASP A01:2021-Broken Access Control. The OWASP site for this flaw describes that "access control enforces policy such that users cannot act outside of their intended permissions". My project has access control vulnerability called an elevation of privileges, which means acting as a user without being logged in or acting as an admin when logged in as a user.

The polling app uses Django's basic authentication backend with Django users database. I have created superuser admin and user bob. The app has a broken access control flaw in source file "views.py" in function [def index(request)](https://github.com/leftidev/django-polling-app/blob/65fbb9a125008a55220ab8da0302dbf14d7d859f/pollingapp/views.py#L11). The app should only allow logged in users to view the page /polls/. The flaw allows acting as a user without being logged in. 

The fix for the broken access control flaw is to add authentication via Django's authentication system, where authenticate() is used to verify a set of credentials. It takes credentials as keyword arguments, username and password, checks them against each authentication backend, and returns a User object if the credentials are valid for a backend. In the project, the valid username is "bob" and valid password is "squarepants". With "user = authenticate()" and the following if-else commented out, there is no authentication at all when viewing the page /polls/.
