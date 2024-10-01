To narzędzie, które umożliwia tworzenie, uruchamianie i zarządzanie aplikacjami w izolowanych środowiskach zwanych **kontenerami**.

**Kontener** to jak mały, samodzielny "mini-komputer" wewnątrz twojego systemu. W kontenerze umieszczasz aplikację oraz wszystko, co jest jej potrzebne (biblioteki, konfiguracje, narzędzia), tak aby mogła działać niezależnie od tego, w jakim środowisku zostanie uruchomiona.

Docker umożliwia uruchamianie wielu kontenerów na jednym systemie, przy czym każdy kontener jest odizolowany od innych. Dzięki temu unikamy problemów z konfliktem zależności, które mogą wystąpić podczas instalacji aplikacji bezpośrednio na systemie.

**Docker Image**: To szablon (obraz) zawierający aplikację i wszystko, co jest jej potrzebne do działania.

**Docker Container**: Kiedy uruchamiasz obraz, tworzysz kontener. Kontener to działający przykład obrazu – aplikacja działa w swoim odizolowanym środowisku.

**Docker Hub**: To rejestr kontenerów, w którym można przechowywać i udostępniać obrazy.

**Wolumeny** (**volumes**) to mechanizm służący do trwałego przechowywania danych generowanych przez kontenery. Dane zapisane bezpośrednio w kontenerze są tymczasowe i zostaną utracone po zatrzymaniu lub usunięciu kontenera. Wolumeny rozwiązują ten problem, pozwalając na przechowywanie danych poza kontenerem, co umożliwia ich dalsze używanie, nawet po odtworzeniu kontenera.

**Docker Compose** to narzędzie, które ułatwia definiowanie i zarządzanie aplikacjami składającymi się z **wielu kontenerów Dockerowych**. Docker sprawdza się w uruchamianiu pojedynczych kontenerów, ale w rzeczywistych projektach aplikacje często składają się z kilku usług działających razem (np. serwer webowy, baza danych, cache). Docker Compose pozwala zarządzać tymi wszystkimi usługami za pomocą jednego pliku konfiguracyjnego.


Przydatne komendy:
* `docker ps` - wyświetla listę uruchomionych kontenerów
* `docker ps -a` - Wyświetla wszystkie kontenery, nie tylko uruchomione, ale także zatrzymane.
* `docker images` - wyświetla listę dostępnych obrazów
* `docker volume ls` - wyświetla listę wolumenów
  

**Porty w Dockerze**:  W przypadku Dockera, porty działają podobnie, ale kontenery są izolowane od systemu hosta. Każdy kontener Dockerowy ma swoje wewnętrzne porty, ale te porty są niewidoczne na zewnątrz, chyba że je zmapujesz na porty systemu hosta. Jest do konieczne, aby mieć dostęp do kontenera np z poziomu przeglądarki.