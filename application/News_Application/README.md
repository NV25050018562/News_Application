# News_Application

# Installation
1. download the zipper file
2. python -m pip install django mysqlclient
3. python -m pip install -r requirements.txt
4. python manage.py migrate
5. python manage.py createsuperuser
6. python manage.py runserver




# Creating Users and Assigning Groups
> Users are assigned to Django groups based on role:

reader - can view approved articles and subscribe
journalist - can create articles/newsletters
editor - can approve/edit/delete any article/newsletter

Example in Django shell:

from news.models import CustomUser
from django.contrib.auth.models import Group

user = CustomUser.objects.create_user(username="testreader", password="password123", role="reader")
group = Group.objects.get(name=user.role)
user.groups.add(group)
user.save()

# API Token Authentication
1. Generate a token for a user:

from news.models import CustomUser
from rest_framework.authtoken.models import Token

user = CustomUser.objects.get(username="testreader")
token, _ = Token.objects.get_or_create(user=user)
print("Token:", token.key)


2. Test the API using curl:

curl -H "Authorization: Token YOUR_TOKEN_HERE" http://127.0.0.1:8000/api/subscribed

>> Example response empty if no subscriptions

3. Alternative ways to test:

- run: python manage.py runserver
- login as a reader

Browsable API: Visit http://127.0.0.1:8000/api/subscribed-articles/ in browser login required





## WHAT THE APP DOES:
Use Cases

Reader

-View approved articles

-Subscribe to publishers

-Subscribe to journalists

-Retrieve subscribed articles via API

Journalist

-Create articles

-Edit articles

-Delete articles

-Publish newsletters

Editor

-Review submitted articles

-Approve articles

-Delete articles

Third-Party API Client

-Retrieve articles based on subscriptions


