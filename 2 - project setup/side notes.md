

**`.:/code`** (Bind Mount): Mapowanie lokalnego katalogu z kodem na katalog w kontenerze. Służy głównie do pracy deweloperskiej, aby natychmiast wprowadzać zmiany w kontenerze.

add 
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code


(wersja bez named volume):
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


**`postgres_data:/var/lib/postgresql/data`** (Named Volume): Trwały wolumen zarządzany przez Dockera, służący do przechowywania danych bazy danych, które powinny przetrwać restart lub usunięcie kontenera.


(wersja z named volume):
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



Ruff check i ruff format

mypy

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

