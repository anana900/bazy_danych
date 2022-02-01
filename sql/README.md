# Wstęp

Ściąga oparta głównie o:
- www.samouczekprogramisty.pl
- https://www.w3schools.com/sql/sql_hosting.asp

Przykładowa baza danych do pobrania:
wget https://github.com/SamouczekProgramisty/chinook-database/raw/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite

# SQL

## Język SQL

- SQL to nie baza danych, ale język który pomaga dogadac się z bazą danych
- język deklaratywny - instrukcje opisuje co chcemy zrobić, a nie jak zrobić
- język ten ukrywa jak skonstruowana jest baza, zwraca fnalny wynik
- oparty jest na zapytaniach
- wielkość liter nie ma znaczenia, chyba że są w cudzysłowiach

## Baza Danych
https://www.samouczekprogramisty.pl/wstep-do-relacyjnych-baz-danych/

- zbiór danych zorganizowany w określony sposób, zapisanych w okreslonym formacie
- sposób organizacji bazy danych decyduje szybkości (zapisu, odczyty, modyfikacji, usuwania) oraz o sposobie dostępu

### Relacyjne Bazy Danych

- zbudowana za pomocą krotek. Krotki mają atrybuty. Każda krotka zapisana jest w relacji
- kazda krtotka jest unikalna - ma co najmniej różny PRIMARY ID
- bazy relacyjne zorganizowane sa w postaci tabel
- relację lub inaczej tabelę w bazie danych można porównać do Klasy
- krotkę można porównać do instancji Klasy, a tym samym do wiersza w tabeli bazy
- elementy krotki to atrybuty Klasy
- operacje opare o algebrę relacji
- dostęp do danych za pomocą SQL
- klucz obcy - to referencja do innej tabeli poprzez jej PRIMARY ID

Relacja == Tabela
Krotka == Wiersz

Rodzaje powiązań
- jeden do jednego - jedno auto jeden nr rejestracyjny.
- jeden do wielu - jeden producent aut, wiele aut
- wiele do wielu - wielu producentów podzespołów zaopatruje wielu producentów aut

Iloczyn kartezjański
tabela A poziada 3 elementy: osa, biedronka, pszczoła
tabele B posiada 2 elementy: czerwony, żółty
Iloczynem kartezjańsim tych dwóch tabel będzie wynikowa tabela z 6 elementami:
osa czerwony
osa żółty
biedronka czerwony
biedronka żółty
pszczoła czerwony
pszczoła żółty

Taki iloczyn można stworzyć używając SQL: SELECT * from A, B;

Najpopularniejsze relacyjne bazy danych:
- PostgreSQL
- MySQL
- SQLite
- Oracle
- SQL Server
- HyperSQL
Różnią się między soba implementacją oraz wersją wspieranego SQL'a.

## SQL Grupy Zapytań

- DQL - Data Query Language - pobieranie danych z tabel bazy
- DML - Data Manipulation Language - aktualizacja danych tabel bazy
- DDL - Data Definition Language - tworzenie lub usuwanie obiektów bazy
- DCL - Data Control Language - zarządzanie dostępem do bazy, manipulacja kontami lub prawami dostępu
- TCL - Transaction Control Language

### DQL

- SELECT - pobieranie danych z bazy

### DML

- INSERT - dodanie wierszu w tabeli
- UPDATE - aktualizacja wiersza w tabeli
- DELETE - usunięcie wiersza z tabeli

### DDL

- CREATE - tworzenie obiektu bazy danych
- ALTER - modyfikacja tabeli bazy danych
- DROP - usunięcie obiektu bazy danych, usuwa całątabelę
- TRUNCATE - usuwa wszystkie dane z tabeli bazy

### DCL

- GRANT - nadaje uprawnienia
- REVOKE - usuwa uprawnienia

### TCL

- BEGIN - rozpoczyna transakcję
- COMMIT - zatwierdza transakcję
- ROLLBACK - wycofuje transakcję
- SAVEPOINT - zapisuje punkt przywracania aktualnej transakcji

# Narzędzia Linux

## SQLITE3

1. uruchamiamy: sqlite3
2. otwieramy bazę danych: .open sciezka-do-bazy
3. wyświetlenie tabel: .tables
4. informacje o bazie: .db
5. wyświetlenie schematu tabeli: .schema nazwa_tabeli
6. zapytania, np.: SELECT * FROM tabela;
7. ustawienia np.: .headers on/off

### Tworzenie Bazy Danych
Z poziomu aplikacji sqlite3: sqlite3 nazwabazy.db

### Eksportowanie Do Skryptu SQL
Z poziomu aplikacji sqlite3: sqlite3 nazwabazy.db .dump > text_db.sql

### Generowanie Bazy Danych Na Podstawie Skryptu
Z poziomu aplikacji sqlite3: sqlite3 nazwabazy.db < text_db.sql


# SQL Składnia DQL
## Select
SELECT - pobieranie danych, w nawiasach opcjonalne wyrażenia:
   SELECT ...
(    FROM ...)
(   WHERE ...)
(ORDER BY ...)
(   LIMIT ...) - ogranicza ilość wyników, do stronicowania uzywane
SELECT DISTINCT pozwala eliminować duplikaty w wynikach

### Stronicowanie
LIMIT - określa ilość elementrow do zwrocenia
OFFSET - pozwala na przeskoczenie pewnej liczby wynikow.

### Zmienne Specjalne
\* - zaznacz wszystko

### Filtrowanie
WHERE - filtruje wyniki wyszukiwania. używana w SELECT, UPDATE, INSERT, DELETE

#### Porównania
IS - porównywanie
() - można używac dla zwiększenia czytelności, najwyzszy priorytet
NOT - negacja, ma wyższy priorytet niż AND
AND - logiczne oraz, ma wyższy priorytet niż OR
OR - logiczne lub
\>, <, = <=, >=, != - porównania do cyfr, stringow, dat, itp
== to to samo co =
<> to ot samo co !=

### NULL
NULL - odpowiednik pythonowego None, albo Java null
IS NULL - do porównania z null stosujemy IS

### Zakres
BETWEEN - do określenia zakresu z domknięciem

### Wyra Regularne
LIKE - podobnie jak wyrażenia regularne. Używa % jako * oraz _ jako ?:
  select * from track where name like '%lan%';
ESCAPE - pozwala nawyszukiwanie znaków specjalnych:
  select * from track where name like '%x%%' ESCAPE 'x';

### Lista Warunków
IN - określa listę warunków:
  select * from invoice where billingcountry = 'USA' AND billingcity IN ('Madison','Boston');

### Duplikaty
DISTINCT - eliminuje duplikaty z wyników. W przypadku zwracania 2 lub wiecej kolumn, dane poszczególnych kolumn mogą sie powtarzać, ale wyniki zawsze będa unikalne.
  select distinct billingcountry from invoice;
  select distinct billingcountry, billingcity from invoice;

### Sortowanie
ORDER BY - służy do sortowania wyników
  select distinct billingcountry from invoice ORDER BY billingcountry;
ORDER BY element DESC - odwraca kolejność sortowania.
Sortowanie po wielu elementach podanych po przecinku - najpierw sortowane są elementy pierwszej kolumny, potem elementy wg drugiej kolumny ale tylko w tedy kiedy pojawiły sie duplikaty po pierszym sortowaniu itd
  select distinct billingcountry, billingstate from invoice ORDER BY billingcountry DESC, billingstate;
AS - alias służący do nadawania własnych nazw kolumnom
  SELECT genreid AS 'a', name AS 'b' from genre LIMIT 5;

### Scalanie Łączenie Tabel
#### UNION
Zbieranie wyników z 2 lub więcej tabel
Liczba kolumn musi byc identyczna we wszystkich zapytaniach
UNION - zwraca unikalne
UNION ALL - zwraca duplikaty
  select name as n from genre union all select distinct billingcity from invoice order by n limit 10;
  select name as n from genre union select distinct billingcity from invoice union all select albumid from track order by n DESC limit 11;

#### Iloczyn kartezjański
Tworzenie tabeli wynikowej poprzez kobbinacje każdego elementu tabeli A z każdym elementem tabeli B:
SELECT * from A, B;

### Filtrowanie Grupy
HAVING - działa podobniejak WHERE, ale filtruje dane na poziomie całej grupy. natomiast WHERE działa na poziomie pojedyńczych wierszy.
Przykład 1.2 - filtrowanie po grupach krajów które wydały powyżej 100:
select billingcountry as bc, sum(total) as st from invoice group by bc having st > 100;
Przykład 1.2 - filtrowanie po grupach krajów które wydały powyżej 100, pomijając miasto Berlin:
select billingcountry as bc, sum(total) as st from invoice where billingcity != 'Ottawa' group by bc having st > 100;

## Funkcje

### Długość
Wyznaczenie długości wyrażenia:
SELECT LENGTH('dasdasd');
Pobranie długości elementu z kolumny BillingAddress:
SELECT LENGTH(BillingAddress) FROM invoice LIMIT 5;
Pobranie unikalnych długości i posortowanie ich rosnąco:
SELECT distinct LENGTH(BillingAddress) as XX FROM invoice order by XX;

### Wartość Maksymalna
SELECT MAX(LENGTH(customerid)) FROM invoice;

### Wartość Bezwzględna
ABS
select ABS(-121);

### Zamiana Na Małe Znaki
LOWER
select lower('Jakis Napis');

### Zamiana Na Duże Znaki
UPPER
select upper('Jakis napis');

### Losowa Liczba Całkowita
RANDOM
select random();

### Podzbiór Znaków
SUBSTR(zbiór_znaków, od_litery, ile_znaków)
Przykład
select SUBSTR('jaka to melodia', 1, 2);
Daje w wyniku: ja

### Usunięcie Białych Znaków
TRIM
select TRIM(' co to ma znaczyc     ');

## Grupowanie Wierszy
GROUP BY - pozwala na grupowanie danych. Pobiera z tabeli dane o taim samym kluczu i tworzy z nich grupy. Nastepnie operacja podana w SELECT jest wykonywana osobno na każdej takiej grupie.
Przykład - wyznaczenie maksymalnej faktury dla każdego kraju w tabeli:
select billingcountry, MAX(total) from invoice group by billingcountry;
To samo z posortowaniem i krajami zawierającymi 'land':
select billingcountry, MAX(total) as XX from invoice WHERE billingcountry LIKE '%land%' group by billingcountry ORDER BY XX;
Uzycie kilku kluczy w grupowaniu, w przykładzie :
select LENGTH(billingaddress) as kraj, MIN(total), billingstate from invoice GROUP BY total, billingcity ORDER BY kraj DESC limit 5;

## Funkcje Grupujące

### Wartość Średnia
AVG

### Wartość Minimalna
MIN

### Wartość Maksymalna
MAX

### Suma
SUM - w przypadku samych wartości NULL zwraca NULL
Przykład - suma zamówień niepowtarzalnych:
SELECT sum(distinct total) FROM invoice;

### Total
TOTAL - działa jak SUM, ale w przypadku samych wartości NULL zwraca 0

### Zliczanie Wystapień
COUNT - zlicza wystąpienia elementów które mają wartość inną niż NULL.
Przykład - liczba zamówień niepowtarzalnych
SELECT COUNT(distinct total) FROM invoice;

## Klauzula JOIN

### Inner JOIN
Złączenie które z iloczynu kartezjańskiego wybiera te wiersze dla których spełniony jest warunek. W żadnej z łączonych tabel kolumna użyta do łączenia nie może mieć wartości NULL.
select * from genre INNER JOIN album ON genre.genreid < 3 and album.albumid < 3;

### Outer JOIN
#### LEFT OUTER JOIN
Zwraca wiersze spełniające warunek złączenia + pozostałe wiersze z lewej tabeli
select count(album.artistid), count(genre.name) from genre LEFT OUTER JOIN album ON genre.genreid < 3 and album.albumid < 3;

#### RIGHT OUTER JOIN
Zwraca wiersze spełniające warunek złączenia + pozostałe wiersze z prawej tabeli
Wystarczy zamienić tabele w zaoytaniu z LEFT OUTER JOIN

#### FULL OUTER JOIN
Zwraca wiersze spełniające warunek złączenia + pozostałe wiersze z prawej i lewej tabeli

#### CROSS JOIN
To inaczej iloczyn kartezjański.

##### Skróty JOIN
JOIN to to samo co INNER JOIN
LEFT JOIN to to samo co LEFT OUTER JOIN
RIGHT JOIN to to samo co RIGHT OUTER JOIN
FULL JOIN to to samo co FULL OUTER JOIN
CROSS JOIN to to samo co iloczyn kartezjański

## Podzapytania
To zapytaania umiesczone wewnątrz innego zapytania, z wykożystaniem nawiasów.

### Podzapytanie proste
To takie które można rozbić na oddzielne
select name from artist where artistid IN (Select artistid from album group BY artistid having count(*) > 10);

### Podzapytanie skorelowane
To takie które powiązane jest z głównym zapytaniem poprzez np alias tabeli

# SQL Składnia DDL

## Tworzenie Bazy Danych
CREATE DATABASE nazwa_bazy;

## Usuwanie Bazy Danych
DROP DATABASE nazwa_bazy;

## Tworzenie Kopii Zapasowej Bazy
BACKUP DATABASE nazwa_bazy;

## Tworzenie Tabeli
### Nowej
CREATE TABLE Pieczywo ( 
PieczywoID int,
Nazwa varchar(255),
Typ varchar(255));

### Na Podstawie Innej
CREATE TABLE Tosty AS
SELECT Nazwa, Typ
FROM Pieczywo;

### Constraint
Ograniczanie dotyczące typów danych dla kolumny.
#### NOT NULL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    Age int
);

#### UNIQUE
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    Age int,
    UNIQUE (ID)
);

#### PRIMARY KEY
Określa która kolumna jest kluczem w tabeli. Taka kolumna musi być unikalna i nie może posiadać wartości NULL.
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    Age int,
    PRIMARY KEY (ID)
);

#### FOREGIN KEY
Zapobiega usuwaniu elementów w tabeli jeśli połączone są one z elementami innych tabel.
Takie połączenie realizuje się jako dodatkową kolumnę odnoszącą się do primary key innej tabeli.
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);

TODO

## Usunięcie Tabeli
DROP TABLE nazwa;

## Wyczyszczenie Tabeli
TRUNCATE TABLE nazwa;

## Modyfikacja Tabeli TRUNCATE
### Dodanie Kolumny
ALTER TABLE Pieczywo ADD Cena int;

### Usunięcie Kolumny
ALTER TABLE Pieczywo DROP COLUMN Cena;



# SQL Injection

https://www.samouczekprogramisty.pl/klauzula-where-w-zapytaniach-sql/

# Przykłady Użycia

1.
select BillingCountry from invoice where InvoiceDate >= '2013-12-05 00:00:00' AND invoicedate <= '2013-12-09 00:00:00' AND Total < 10;

2.
select BillingState from invoice where BillingCountry = 'USA' AND Total>15;

3.
select BillingCountry, billingcity 
from invoice where (billingstate is NULL AND total > 17) OR (total < 1 AND billingstate IS NOT NULL AND invoicedate > '2013-09-20 00:00:00');

4.
zwróci wszystkie wiersze z tabeli track, dla których:
unitprice jest mniejsze niż 1 i znak % zawarty jest w kolumnie name 
oraz kolumna name kończy się na e.
select * from track where unitprice < 1 AND name LIKE '%x%%e' ESCAPE 'x';

5.
zwróci wszystkie wiersze z tabeli invoice, które mają uzupełnioną kolumnę 
billingstate i nie są ze Stanów Zjednoczonych
select * from invoice where billingstate is not NULL AND billingcountry != 'USA';

6.
zwróci wszystkie wiersze z tabeli invoice, które dotyczą Polski, Czech 
albo Węgier dla których wartość faktury przekracza 10
select * from invoice where BillingCountry in ('Poland', 'Germany') AND total > 10;

7.
zwróci imiona pracowników z tabeli employee, które dotyczą pracowników 
urodzonych w latach 60.
select FirstName from employee where BirthDate BETWEEN '1960-01-01 00:00:00' AND '1969-12-31 00:00:00';

8.
ograniczenie wyszukiwania do pierwszego wyniku
select FirstName from employee 
where BirthDate BETWEEN '1960-01-01 00:00:00' AND '1969-12-31 00:00:00' Limit 1;

9.
zwróci dziesięć najdłuższych ścieżek (tabela track, kolumna milliseconds), weź pod uwagę tylko te, których kompozytor (kolumna composer) zawiera literę b
select milliseconds,name from track where composer like '%b%' order by milliseconds desc limit 10;

10.
zwróci pięć najtańszych ścieżek (tabela track, kolumna unitprice) dłuższych niż minuta
select unitprice, name from track where milliseconds > 620000 order by unitprice limit 5;

11.
zwróci unikalną listę dziesięciu kompozytorów których ścieżki kosztują mniej niż 2$ posortowanych malejąco według identyfikatora gatunku (kolumna genreid) i rosnąco według rozmiaru (kolumna bytes)
select distinct composer from track where unitprice < 2 order by genreid desc, bytes limit 10;

12.
zwróci piątą stronę z fakturami (tabela invoice) zakładając, że na stronie znajduje się dziesięć faktur i sortowane są według identyfikatora (kolumna invoiceid).
select * from invoice order by invoiceid limit 10 offset 40;

13.
zwróci dwie kolumny. Pierwsza z nich powinna zawierać ścieżki (kolumna name) droższe niż 1$ i poprawnych kompozytorów (kolumna composer nie ma wartości NULL) pod nazwą magic thingy. Druga powinna zawierać liczbę bajtów. Wynik powinien zawierać dziesięć wierszy i być posortowany rosnąco po liczbie bajtów3
select name AS 'magic thingy', bytes from track where unitprice > 1 UNION select composer, bytes from track where composer is not NULL order by bytes limit 10;

14.
średnią, minimalną i maksymalną wartość kolumny total w tabeli invoice
select AVG(total), MAX(total), MIN(total) from invoice;

15.
liczbę wierszy w tabeli invoice w których długość kolumny billingcountry jest większa niż 5
select count(*) from invoice where length(billingcountry) > 5;

16.
liczbę unikalnych dat (kolumna invoicedate), w których wystawiono faktury (tabela invoice)
select count(distinct invoicedate) from invoice;

17.
daty (kolumna invoicedate), w których wystawiono co najmniej dwie faktury (tabela invoice),
select invoicedate as ivd from invoice group by ivd having count(ivd) >= 2;

18.
pięć losowych wierszy z tabeli genre (wywołania tego zapytania wiele razy powinno zwrócić różne wyniki)
select * from genre order by random() limit 5;

19.
miesięczną (kolumna invoicedate) sumę faktur (kolumna total w tabeli invoice) od kupujących z identyfikatorem (kolumna customerid) mniejszym niż 30, wynik powinien być posortowany po miesięcznej sumie faktur i zawierać jedynie te miesiące dla których suma jest większa od 40.
select sum(total) as mt, substr(invoicedate, 1, 7) as ivd from invoice where customerid < 30 group by ivd having mt > 40 order by mt;

20.
liczbę wierszy w iloczynie kartezjańskim tabel track, invoice i invoiceline (UWAGA! to zapytanie może trochę potrwać)
select count(*) from track, invoice, invoiceline;

21.
tytuł albumu (kolumna title w tabeli album) i nazwę artysty (kolumna name w tabeli artist) dla wszystkich nazw artystów zaczynających się od s
select artist.name, album.title from artist JOIN album ON artist.artistid = album.artistid where artist.name LIKE 's%';


