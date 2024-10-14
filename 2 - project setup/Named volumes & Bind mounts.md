**Named volumes** i **bind mounts** to dwa sposoby udostępniania danych pomiędzy kontenerem a hostem w Dockerze.

**Bind mount** pozwala bezpośrednio połączyć katalog na hoście (Twoim komputerze) z katalogiem wewnątrz kontenera.
Bind mounty są szczególnie przydatne w sytuacjach, gdy chcesz, aby aplikacja działająca w kontenerze miała dostęp do plików, które regularnie edytujesz na hoście (np. podczas rozwoju aplikacji).

Masz bazę danych PostgreSQL w jednym kontenerze, która zapisuje dane użytkowników, itd. Gdybyś uruchomił bazę **bez named volume**, po usunięciu kontenera wszystkie dane by zniknęły. Named volume działa jak "bezpieczny magazyn", który Docker tworzy i zarządza nim samodzielnie, przechowując dane w bezpiecznym miejscu.

```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_USER: eventilo
      POSTGRES_PASSWORD: eventilo
      POSTGRES_DB: eventilo
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  api:
    build: .
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:
```

#### Co tu się dzieje?

- **`postgres_data:/var/lib/postgresql/data`** to **named volume**. Oznacza to, że Docker tworzy specjalne miejsce na dane PostgreSQL w domyślnej lokalizacji na hoście.
- Dane bazy PostgreSQL są **przechowywane na tym named volume**, więc nawet jeśli usuniesz kontener PostgreSQL, dane nie znikną. Będą przechowywane bezpiecznie na woluminie.

- **`.:/code`** to **bind mount**. Oznacza to, że obecny katalog na Twoim komputerze (hoście) jest bezpośrednio podłączony do katalogu `/code` wewnątrz kontenera.
- Każda zmiana, którą zrobisz w plikach w obecnym katalogu (`.`) na swoim komputerze, od razu pojawi się w kontenerze (bez potrzeby przebudowywania obrazu).