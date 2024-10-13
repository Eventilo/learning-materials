* narzędzie służące do statycznej kontroli typów w Pythonie.
* Python jest językiem dynamicznie typowanym, co oznacza, że typy zmiennych są określane w trakcie działania programu, a nie podczas jego kompilacji.
* `Mypy` pozwala na dodanie opcjonalnej kontroli typów w Pythonie, co może poprawić jakość kodu, ułatwić jego zrozumienie oraz wykryć potencjalne błędy już na etapie tworzenia.

```python
def add_numbers(a: int, b: int) -> int:
	return a + b
```
Jeśli spróbujesz przekazać do tej funkcji coś innego, np. `str`, `Mypy` wskaże na potencjalny problem jeszcze przed uruchomieniem programu.


Aby sprawdzić zgodność typów w pliku, uruchamiasz:
```bash
mypy nazwa_pliku.py
```
