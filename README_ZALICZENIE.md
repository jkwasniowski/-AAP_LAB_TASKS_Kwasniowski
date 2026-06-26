# Sprawozdanie — zestaw zaliczeniowy AAP

## Cel

Celem zestawu było połączenie tematów z laboratoriów w jeden prosty pipeline analityczny w Pythonie.

## Wykonane zadania

W pierwszej części zrobiłem dekoratory `retry` i `cache_to_disk`. `retry` ponawia wywołanie funkcji po błędzie, a `cache_to_disk` zapisuje wynik do pliku JSON. Przy 5 próbach skuteczność teoretyczna wynosiła około 96,9%, a w teście wyszło 96 sukcesów na 100 wywołań. Cache również zadziałał poprawnie.

W części o współbieżności porównałem wykonanie sekwencyjne, `ThreadPoolExecutor` i `multiprocessing.Pool`. Dla prostego liczenia sentymentu wyniki były takie same we wszystkich wariantach. Najszybszy okazał się multiprocessing.

W części testowej przygotowałem klasę `Tokenizer` i testy w `pytest`. Testy sprawdzają między innymi puste teksty, HTML, wielkość liter, interpunkcję, polskie znaki i filtr minimalnej długości tokenu. Wynik testów: 10 testów przeszło, 1 był oznaczony jako oczekiwane niepowodzenie.

W części bazodanowej zapisałem recenzje do SQLite w dwóch wariantach: klasyczna tabela SQL oraz tabela z dokumentem JSON. SQL był czytelniejszy i szybszy przy agregacjach.

W części PySpark użyłem DataFrame, funkcji okienkowych i obliczeń na recenzjach. Policzony został ranking najdłuższych recenzji w obrębie klasy, różnica od średniej długości recenzji w klasie oraz średnia krocząca dla 50 ostatnich recenzji. Spark poprawnie wykonał agregacje dla obu klas.

W ostatniej części przygotowałem prosty kontrakt jakości danych. Sprawdzane były między innymi braki danych, poprawność etykiet, minimalna i maksymalna długość recenzji, duplikaty oraz balans klas. Wszystkie reguły krytyczne przeszły poprawnie. Reguła dotycząca tagów HTML zwróciła ostrzeżenie, ponieważ w danych IMDb dużo recenzji zawiera znaczniki typu `<br />`.

## Najważniejsze wyniki

- próbka 2000 recenzji była równo zbalansowana: 1000 negatywnych i 1000 pozytywnych,
- średnia długość recenzji wynosiła około 225 słów dla klasy negatywnej i 232 słowa dla pozytywnej,
- testy `pytest` dla tokenizatora zakończyły się wynikiem: 10 passed, 1 xfailed,
- w walidacji danych nie było nulli ani duplikatów,
- wykryto 1189 recenzji z tagami HTML, ale była to reguła ostrzegawcza, więc nie zatrzymywała programu.

## Wnioski

Najbardziej praktyczne okazały się dekoratory do ponawiania i cache, testy automatyczne oraz walidacja danych. Widać też, że przed analizą tekstu trzeba uważać na jakość danych, bo nawet popularny zbiór IMDb zawiera dużo pozostałości HTML. Dla prostych agregacji lepiej sprawdził się klasyczny SQL, natomiast PySpark jest przydatny przy większych danych i bardziej złożonych analizach, takich jak ranking czy średnie kroczące.
