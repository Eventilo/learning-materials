### **Podstawowe komendy**
- `ls` - listowanie plików i katalogów
- `mkdir` - tworzenie nowego katalogu
- `cd` - zmiana katalogu
* `sudo` - uruchomienie z uprawnieniami administratora

---
### **Zarządzanie pakietami**
### 1. APT
APT pobiera i instaluje pakiety ze standardowych repozytoriów, które są specyficzne dla danej wersji systemu. Pakiety są zazwyczaj zależne od wersji systemu i jego bibliotek.

```bash
 apt install NAZWA_PAKIETU
```

### 2. SNAP
Snapy są samowystarczalne, zawierają wszystkie zależności, więc działają niezależnie od wersji systemu.

```bash
snap install NAZWA_PAKIETU
```

------
### **Uprawnienia**
Każdy plik ma przypisane uprawnienia dla trzech kategorii użytkowników:

1. **Właściciel** (użytkownik) – Osoba, która jest właścicielem pliku.
2. **Grupa** – Grupa użytkowników, którzy mogą mieć przypisane wspólne uprawnienia.
3. **Inni** – Wszyscy inni użytkownicy, którzy nie są właścicielem ani w grupie.

Uprawnienia są wyrażane w postaci:

- **r** – Czytanie (read): Pozwala na przeglądanie zawartości pliku/katalogu.
- **w** – Zapis (write): Pozwala na modyfikację zawartości pliku lub zmianę katalogu (dodawanie/usuwanie plików).
- **x** – Wykonanie (execute): Pozwala uruchamiać plik jako program (w przypadku pliku) lub przemieszczać się do katalogu (w przypadku katalogu).

Komenda **`ls -l`** w terminalu pokazuje szczegółową listę plików i katalogów w bieżącym katalogu, w tym informacje o uprawnieniach.

```bash
$ ls -l
drwxr-xr-x  2 user group 4096 Sep 30 10:30 katalog1
-rw-r--r--  1 user group  123 Sep 30 10:31 plik1.txt
```

1. Uprawnienia
```
   drwxr-xr-x
   -rw-r--r--
```
2. Liczba linków (hard links):
```
   2
   1
```
3. Właściciel i grupa:
```
   user group
   user group
```
4. Rozmiar pliku:
```
   4096
   123
```
5. Data ostatniej modyfikacji:
```
   Sep 30 10:30
   Sep 30 10:31
```
6. Nazwa pliku lub katalogu:
```
   katalog1
   plik1.txt
```

Wynik `drwxr-xr-x` dla **katalogu** oznacza:

- **d**: Jest to katalog.
- **rwx**: Właściciel ma uprawnienia do czytania, zapisu i wykonania.
- **r-x**: Grupa ma uprawnienia do czytania i wykonania, ale nie może zapisywać.
- **r-x**: Inni użytkownicy mają uprawnienia do czytania i wykonania, ale nie mogą zapisywać.

Wynik `-rw-r--r--` dla **pliku** oznacza:

- **-**: Jest to plik.
- **rw-**: Właściciel może czytać i zapisywać plik, ale nie może go wykonać.
- **r--**: Grupa może tylko czytać plik.
- **r--**: Inni użytkownicy mogą tylko czytać plik.

---
### **Grupy**
Sposób organizowania użytkowników, aby łatwiej zarządzać uprawnieniami do plików i zasobów.

- **Grupa** to zestaw użytkowników, którzy dzielą wspólne uprawnienia. Jeśli plik jest przypisany do grupy, każdy użytkownik w tej grupie może mieć uprawnienia do tego pliku.
- Każdy użytkownik może należeć do jednej lub więcej grup.
  
- **`groups`** – Pokaż grupy, do których należy użytkownik.
- **`groupadd <nazwa_grupy>`** – Tworzenie nowej grupy.
- **`usermod -aG <grupa> <użytkownik>`** – Dodanie użytkownika do grupy.
---
### **Porty**
Służą do komunikacji między różnymi programami oraz między komputerem a innymi urządzeniami w sieci. Każdy port służy do rozpoznawania konkretnej usługi lub aplikacji.

Kiedy komputer wysyła lub odbiera dane przez sieć, każda taka operacja odbywa się na określonym porcie. Na przykład:

- Przeglądarka internetowa korzysta z portu **80** (dla HTTP) lub **443** (dla HTTPS) do komunikacji z serwerami WWW.
- Aplikacje takie jak SSH używają portu **22** do bezpiecznego połączenia zdalnego.

Porty działają w połączeniu z adresem IP, który identyfikuje urządzenie w sieci. Adres IP to jak **adres budynku**, a port to **numer konkretnego pokoju**, do którego skierowane są dane.

---
### **Gniazda (Sockets)**
Skłąda się z **adresu IP**, **numeru portu** oraz **protokółu** (np. TCP lub UDP).

Jest punktem końcowym połączenia sieciowego. Tworzy ono połączenie między dwoma urządzeniami w sieci (np. komputerem-klientem i serwerem). Dzięki gniazdom komputer może wymieniać dane z innymi urządzeniami w sieci.
**Gniazdo** można rozumieć jako połączenie między adresami IP i portami na obu końcach komunikacji sieciowej, czyli między klientem a serwerem.

To połączenie adresu IP (np. `192.168.1.100`), portu (np. `80`) i protokołu (np. TCP), które tworzy punkt końcowy do przesyłania danych między dwoma urządzeniami.
