# Ordinacija Porodicne Medicine

# 1. ER Dijagram
![AAAa](https://github.com/JovanJevtic/aa/blob/main/IMG.jpg)

# 2. Tipovi Podataka

> Prikazati korištenje sljedećih tipova podataka - datumski (u minimalno 3 entiteta), alfanumerički, numerički (cjelobrojni i decimalni) i dr.

Datumski[Pacijent, Zakazivanje, Doktor..]
Numericki[
    int: [Pacijent - BrojPregleda]
    float: [Pregled - Cijena]
]...

# 3 Integriteti

> Uspostaviti integritete (na nivou entiteta, domena i referencijalnosti)

Integritet domena: Pol('Muski' | 'Zenski')
Integritet entiteta: Primarni kljuc
Integritet referencijalnosti: Strani kljuc (FK)

# 4 Obezbjediti potrebne podatke
`INSERT INTO`