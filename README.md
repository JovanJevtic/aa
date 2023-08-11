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
Integritet referencijalnosti: Strani kljuc (FK)\

# 4 Obezbjediti potrebne podatke
`INSERT INTO`

# 5 Prikazati koristenje SQL upita: DMS i DDS naredbe

## DMS / DML (Data Manipulation) 

### 1. Select\

`SELECT Ime, Prezime FROM Pacijent;`
### 2. INSERT\

`INSERT INTO Pacijent (Ime, Prezime) VALUES ('Marko', 'Marković');`
### 3. UPDATE\

`UPDATE Pacijent SET Ime = 'Ana' WHERE PacijentID = 1;`
### 4. DELETE
`DELETE FROM Pacijent WHERE PacijentID = 2;`

## DDS / DDL (Data Definition Language) naredbe:

### 1. CREATE TABLE: Kreira novu tabelu.\
`
CREATE TABLE Doktor (
    DoktorID INT PRIMARY KEY,
    Ime VARCHAR(50),
    Specijalnost VARCHAR(50)
);
`

### 2. ALTER TABLE: Menja strukturu postojeće tabele.\
`ALTER TABLE Pacijent ADD DatumRodjenja DATE;`

### 3. DROP TABLE: Briše tabelu.\
`DROP TABLE Sestra;`