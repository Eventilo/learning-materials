1. Stwórz folder z projektem
2. Stwórz w nim Dockerfile
```Dockerfile
FROM python:3.12-slim  
# Używa obrazu bazowego Pythona w wersji 3.12 w wersji "slim", która jest lżejsza i zajmuje mniej miejsca niż pełne obrazy.

WORKDIR /code  
# Ustawia katalog roboczy wewnątrz kontenera na "/code". Wszystkie kolejne polecenia będą wykonywane w tym katalogu.

RUN pip install --no-cache-dir poetry  
# Instaluje menedżera zależności "Poetry" za pomocą pip. Opcja `--no-cache-dir` zapobiega zapisywaniu tymczasowych plików instalacyjnych, co oszczędza miejsce w obrazie.

COPY pyproject.toml poetry.lock* ./  
# Kopiuje pliki `pyproject.toml` oraz opcjonalnie `poetry.lock` (jeśli istnieje) z lokalnego systemu do katalogu roboczego kontenera (`/code`). Te pliki zawierają informacje o zależnościach projektu.

RUN poetry config virtualenvs.create false  
# Konfiguruje Poetry, aby nie tworzył wirtualnego środowiska. Wszystkie zależności będą instalowane globalnie (wewnątrz systemowego środowiska kontenera).

RUN poetry install  
# Instaluje wszystkie zależności projektu, zgodnie z informacjami z plików `pyproject.toml` i `poetry.lock`.

COPY . .  
# Kopiuje wszystkie pozostałe pliki z lokalnego katalogu do katalogu roboczego kontenera (`/code`), aby kontener miał dostęp do pełnego kodu aplikacji.

```
3. Stwórz docker-compose.yml
```yml
services:
  db:
    image: postgres:17  
    # Definiuje usługę o nazwie "db" opartą na oficjalnym obrazie Postgres w wersji 17.

    environment:
      POSTGRES_USER: eventilo  
      # Ustawia zmienną środowiskową `POSTGRES_USER`, która określa nazwę użytkownika w bazie danych na "eventilo".
      
      POSTGRES_PASSWORD: eventilo  
      # Ustawia zmienną środowiskową `POSTGRES_PASSWORD`, która określa hasło użytkownika bazy danych na "eventilo".
      
      POSTGRES_DB: eventilo  
      # Ustawia zmienną środowiskową `POSTGRES_DB`, która tworzy bazę danych o nazwie "eventilo" po uruchomieniu kontenera.

    ports:
      - "5432:5432"  
      # Mapuje port kontenera 5432 (domyślny port Postgres) na port hosta 5432, co pozwala na dostęp do bazy danych z zewnątrz kontenera.

    volumes:
      - postgres_data:/var/lib/postgresql/data  
      # Tworzy trwały wolumin `postgres_data`, który jest montowany w katalogu `/var/lib/postgresql/data` wewnątrz kontenera. 
      # Zapewnia trwałe przechowywanie danych bazy danych, nawet po zatrzymaniu kontenera.

  api:
    build: .  
    # Definiuje usługę "api", która buduje obraz z bieżącego katalogu (`.`), korzystając z pliku Dockerfile.

    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload  
    # Określa komendę do uruchomienia aplikacji FastAPI za pomocą serwera Uvicorn. 
    # Serwer nasłuchuje na adresie 0.0.0.0 (wszystkie interfejsy) i na porcie 8000, z włączoną opcją "reload" dla automatycznego przeładowania podczas zmian w kodzie.

    volumes:
      - .:/code  
      # Montuje bieżący katalog (`.`) hosta do katalogu `/code` wewnątrz kontenera, co pozwala na bieżące wprowadzanie zmian w kodzie bez konieczności odbudowywania obrazu.

    ports:
      - "8000:8000"  
      # Mapuje port 8000 w kontenerze na port 8000 na hoście, umożliwiając dostęp do aplikacji API z zewnątrz kontenera.

    depends_on:
      - db  
      # Określa zależność, że kontener "api" zależy od kontenera "db". 
      # Docker Compose uruchomi kontener "db" przed próbą uruchomienia "api".

volumes:
  postgres_data:  
  # Definiuje nazwany wolumin "postgres_data", który będzie używany przez usługę "db" do trwałego przechowywania danych bazy danych.

```
4. poetry init
5. poetry add fastapi uvicorn ruff mypy
6. Stwórz folder app i plik main.py
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```
7. Zbuduj i uruchom kontenery poleceniem `docker compose up --build`

Przydatne komendy:
* `docker ps`  wyświetla aktualnie działające kontenery
* `docker ps -a` wyświetla działające i zatrzymane kontenery
* `docker rm` usuwa zatrzymany kontener
* `docker images` wyświetla obrazy
* `docker rmi` obrazy
* `docker exec -it NAZWA_KONTENERA bash` wejście do kontenera
* `docker volume ls` wyświetla wolumeny
* `docker volume rm` usuwa wolumen

* `poetry init` aktywacja poetry w folderze
* `poetry add` dodanie zależności
* `poetry install` instalacja zależności (add robi to automatycznie)
* `poetry shell` aktywacja wirtualnego środowiska 

* `ruff check .` sprawdza wszystkie pliki w bieżącym katalogu (.) pod kątem problemów z kodem.
* `ruff format .` formatowanie wszystkich plików w bieżącym katalogu (`.`), poprawiając błędy stylistyczne.
* `mypy .` uruchomi analizę typów dla wszystkich plików w bieżącym katalogu.