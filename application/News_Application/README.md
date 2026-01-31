# News_Application

A Django-based news application with role-based access control (Reader, Journalist, Editor) for managing articles, newsletters, and subscriptions.

---

## Installation

### Option 1: Docker Installation (Recommended)

#### Prerequisites
- Docker and Docker Compose installed

#### Steps
1. Clone or download the project
2. Navigate to the project directory
3. Run the application:
   ```bash
   docker-compose up
   ```
4. Create a superuser (in another terminal):
   ```bash
   docker-compose exec web python manage.py createsuperuser
   ```
5. Access the application at `http://localhost:8000`

**Database**: MySQL will automatically start and initialize. Credentials can be found in `docker-compose.yml`

---

### Option 2: Local Installation

#### Prerequisites
- Python 3.11+
- MySQL 8.0+

#### Steps
1. Download the project
2. Install Django and MySQL client:
   ```bash
   python -m pip install django mysqlclient
   ```
3. Install dependencies:
   ```bash
   python -m pip install -r requirements.txt
   ```
4. Configure database in `News_Application/settings.py` (ensure MySQL is running)
5. Run migrations:
   ```bash
   python manage.py migrate
   ```
6. Create a superuser:
   ```bash
   python manage.py createsuperuser
   ```
7. Start the development server:
   ```bash
   python manage.py runserver
   ```
8. Access the application at `http://127.0.0.1:8000`

---

## User Management
### Creating Users and Assigning Groups

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

---

## API Token Authentication
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

---

## Application Features

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

