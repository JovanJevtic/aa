# Ordinacija Porodicne Medicine

# 1. ER Dijagram
![AAAa](https://github.com/JovanJevtic/aa/blob/main/IMG.jpg)

# 2. Tipovi Podataka

> Prikazati korištenje sljedećih tipova podataka - datumski (u minimalno 3 entiteta), alfanumerički, numerički (cjelobrojni i decimalni) i dr.

Datumski[Pacijent, Zakazivanje, Doktor..]\
Numericki[
    int: [Pacijent - BrojPregleda]
    float: [Pregled - Cijena]
]\
...

# 3 Integriteti

> Uspostaviti integritete (na nivou entiteta, domena i referencijalnosti)

Integritet domena: Pol('Muski' | 'Zenski')\
Integritet entiteta: Primarni kljuc\
Integritet referencijalnosti: Strani kljuc (FK)

# 4 Obezbjediti potrebne podatke
`INSERT INTO`

# 5 Prikazati koristenje SQL upita: DMS i DDS naredbe

## DMS / DML (Data Manipulation) 

### 1. Select

`SELECT Ime, Prezime FROM Pacijent;`
### 2. INSERT

`INSERT INTO Pacijent (Ime, Prezime) VALUES ('Marko', 'Marković');`
### 3. UPDATE

`UPDATE Pacijent SET Ime = 'Ana' WHERE PacijentID = 1;`
### 4. DELETE
`DELETE FROM Pacijent WHERE PacijentID = 2;`

## DDS / DDL (Data Definition Language) naredbe:

### 1. CREATE TABLE: Kreira novu tabelu.
`
CREATE TABLE Doktor (
    DoktorID INT PRIMARY KEY,
    Ime VARCHAR(50),
    Specijalnost VARCHAR(50)
);
`

### 2. ALTER TABLE: Menja strukturu postojeće tabele.
`ALTER TABLE Pacijent ADD DatumRodjenja DATE;`

### 3. DROP TABLE: Briše tabelu.
`DROP TABLE Sestra;`

# 6 Prikazati koristenje funkcija

## 1. Datumske funkcije:

### 1. DATEADD: Dodavanje određenog intervala vremena datumu.
`SELECT DATEADD(DAY, 7, GETDATE()) AS SevenDaysFromNow;`

### 2. DATEDIFF: Računanje razlike između dva datuma u određenim intervalima.
`SELECT DATEDIFF(YEAR, DatumRodjenja, GETDATE()) AS GodinePacijenta FROM Pacijent;`

## 2. Funkcije za rad sa stringovima:

### 1. CONCAT: Spajanje dva ili više stringa.
`SELECT CONCAT(Ime, ' ', Prezime) AS PunoIme FROM Pacijent;`

## 3. Agregatne funkcije:

### 1. COUNT: Brojanje redova u rezultatu.
`SELECT COUNT(*) AS BrojPacijenata FROM Pacijent;`

### 2. SUM: Sabiranje vrednosti u određenoj koloni.
`SELECT SUM(Iznos) AS UkupnaVrednost FROM Faktura;`

## 4. Numeričke funkcije:

### 1. ROUND: Zaokruživanje broja na određeni broj decimala.
`SELECT Iznos, ROUND(Iznos, 2) AS ZaokruzenIznos FROM Faktura;`

## 5. Opšte namjene funkcije:

### 1. COALESCE: Vraća prvu ne-NULL vrednost iz liste.
`SELECT Ime, COALESCE(Nadimak, 'Nepoznat') AS PrikazNadimka FROM Pacijent;`

# 7 Koristenje pogleda, uskladistenih funkcija, triggera, udf funkcija

> Prikazati korištenje pogleda, uskladištenih procedura, trigera i UDF funkcija izradom minimalno po dva objekta za svaki od navedenih u ovoj tački

### 1. Pogledi 
#### Pogled za listu pacijenata sa imenima doktora:

`
CREATE VIEW PregledPacijenata AS
SELECT p.Ime AS ImePacijenta, d.Ime AS ImeDoktora
FROM Pacijent p
JOIN Pregled pr ON p.PacijentID = pr.PacijentID
JOIN Doktor d ON pr.DoktorID = d.DoktorID;
`
#### Pogled za prosečne godine pacijenata:
CREATE VIEW ProsečneGodine AS
SELECT AVG(DATEDIFF(YEAR, DatumRodjenja, GETDATE())) AS ProsečneGodinePacijenata
FROM Pacijent;

### 2. Uskladištene procedure (Stored Procedures):

#### Uskladištena procedura za dodavanje novog pacijenta:
`
CREATE PROCEDURE DodajPacijenta
    @Ime VARCHAR(50),
    @Prezime VARCHAR(50),
    @DatumRodjenja DATE
AS
BEGIN
    INSERT INTO Pacijent (Ime, Prezime, DatumRodjenja)
    VALUES (@Ime, @Prezime, @DatumRodjenja);
END;
`

#### Uskladištena procedura za ažuriranje imena pacijenta:
`
CREATE PROCEDURE AzurirajImePacijenta
    @PacijentID INT,
    @NovoIme VARCHAR(50)
AS
BEGIN
    UPDATE Pacijent
    SET Ime = @NovoIme
    WHERE PacijentID = @PacijentID;
END;
`

### 3. Triggeri

#### Triger za automatsko ažuriranje datuma poslednje izmene pacijenta:
`
CREATE TRIGGER UpdateDatumIzmene
ON Pacijent
AFTER UPDATE
AS
BEGIN
    UPDATE p
    SET DatumPoslednjeIzmene = GETDATE()
    FROM Pacijent p
    JOIN INSERTED i ON p.PacijentID = i.PacijentID;
END;
`
#### Triger za sprečavanje brisanja pacijenta koji ima pregled:
`
CREATE TRIGGER PreventDeleteWithPregled
ON Pacijent
FOR DELETE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM deleted d JOIN Pregled pr ON d.PacijentID = pr.PacijentID)
    BEGIN
        RAISEERROR('Pacijent ima zakazane preglede i ne može se obrisati.', 16, 1);
        ROLLBACK TRANSACTION;
    END;
END;`

### 4. UDF (User-Defined Function) funkcije

#### UDF funkcija za računanje starosti pacijenta:
`
CREATE FUNCTION StarostPacijenta (@DatumRodjenja DATE)
RETURNS INT
AS
BEGIN
    RETURN DATEDIFF(YEAR, @DatumRodjenja, GETDATE());
END;
`
#### UDF funkcija za spajanje imena i prezimena:
`
CREATE FUNCTION SpojiImePrezime (@Ime VARCHAR(50), @Prezime VARCHAR(50))
RETURNS VARCHAR(100)
AS
BEGIN
    RETURN CONCAT(@Ime, ' ', @Prezime);
END;
`