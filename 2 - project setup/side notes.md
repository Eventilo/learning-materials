# (previous week)

**`.:/code`** (Bind Mount): Mapowanie lokalnego katalogu z kodem na katalog w kontenerze. Służy głównie do pracy deweloperskiej, aby natychmiast wprowadzać zmiany w kontenerze.

add 
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code



```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_USER: eventilo
      POSTGRES_PASSWORD: eventilo
      POSTGRES_DB: eventilo

  api:
    build: .
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```

```shell
docker compose up --build
docker ps
docker exec -it ... bash
psql -U eventilo
```

```sql
CREATE TABLE cars (  
  brand VARCHAR(255),  
  model VARCHAR(255),  
  year INT  
);
```

docker stop rm 
docker compose up --build

**`postgres_data:/var/lib/postgresql/data`** (Named Volume): Trwały wolumen zarządzany przez Dockera, służący do przechowywania danych bazy danych, które powinny przetrwać restart lub usunięcie kontenera.


```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_USER: eventilo_hub
      POSTGRES_PASSWORD: eventilo_hub
      POSTGRES_DB: eventilo_hub
    volumes:
      - postgres_data:/var/lib/postgresql/data

  api:
    build: .
    command: uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:

```


# This week


Ruff check i ruff format

mypy i funkcja z dodawaniem (plus flaga strict)

konfiguracja GH actions

.github/workflows/ci.yml

```yml
name: CI

on:
    push:
      branches: [ main ]
    pull_request:
      branches: [ main ]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
        - name: Checkout repository
          uses: actions/checkout@v4

        - name: Set up Python
          uses: actions/setup-python@v4
          with:
            python-version: '3.12'

        - name: Install dependencies
          run: |
            sudo apt-get update
            python -m pip install --upgrade pip
            pip install poetry
            poetry install

        - name: Run ruff check
          run: |
            poetry run ruff check .

        - name: Run ruff format check
          run: |
            poetry run ruff format --check .

        - name: Run mypy
          run: |
            poetry run mypy .

```



poetry add sqlalchemy alembic psycopg2-binary


moduł database i plik connection.py i model base declarative base orm


conncetion.py
```python
from sqlalchemy import create_engine
from .config import settings

DATABASE_URL = "postgresql+psycopg2://{user}:{password}@{host}:{port}/{name}".format(
    user="week_2",
    password="week_2",
    host="db",
    port=5432,
    name="week_2",
)

engine = create_engine(DATABASE_URL)
```


```python
from sqlalchemy.orm import DeclarativeBase


class Base(DeclarativeBase):
    pass

```


create users model with username


```python
from sqlalchemy import Integer, String
from sqlalchemy.orm import mapped_column, Mapped
from app.database.model import Base


class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    username: Mapped[str] = mapped_column(String, unique=True)
```

alembic init alembic

w run_migrations_offline podmieć url bazy

w online:
configuration = config.get_section(config.config_ini_section)
configuration["sqlalchemy.url"] = DATABASE_URL


poetry

```
from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    DATABASE_NAME: str = "week_2"
    DATABASE_USER: str = "week_2"
    DATABASE_PASSWORD: str = "week_2"
    DATABASE_HOST: str = "db"
    DATABASE_PORT: str = "5432"

    model_config = SettingsConfigDict(env_file=".env", extra="ignore")


settings = Settings()

```