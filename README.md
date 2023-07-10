# JAVA-S3-D1

Esercizio #1

Creo 4 TABLE:

CREATE TABLE clienti (
    numero_cliente SERIAL PRIMARY KEY,
    nome VARCHAR(50),
    cognome VARCHAR(50),
    data_nascita DATE,
    regione VARCHAR(50),
    residenza VARCHAR(100)
);

CREATE TABLE fatture (
    numero_fattura SERIAL PRIMARY KEY,
    tipologia VARCHAR(50),
    importo DOUBLE(10,2),
    iva DOUBLE(5,2),
    id_cliente INTEGER REFERENCES clienti(numero_cliente),
    data_fattura DATE,
    numero_fornitore INTEGER  INTEGER REFERENCES fornitori(numero_fornitore);
);


CREATE TABLE prodotti (
    id_prodotto SERIAL PRIMARY KEY,
    descrizione VARCHAR(100),
    in_produzione BOOLEAN,
    in_commercio BOOLEAN,
    data_attivazione DATE,
    data_disattivazione DATE
);


CREATE TABLE fornitori (
    numero_fornitore SERIAL PRIMARY KEY,
    denominazione VARCHAR(100),
    regione_residenza VARCHAR(50)
);



Estrarre nome e il cognome dei clienti nati nel 1982

SELECT nome, cognome
FROM clienti
WHERE EXTRACT(YEAR FROM data_nascita) = 1982




Estrarre il numero di fatture con iva al 20%


SELECT COUNT(*) AS numero_fatture
FROM Fatture
WHERE iva = 20


Riportare il numero di fatture e la somma dei relativi importi divisi per anno di fatturazione


SELECT EXTRACT(YEAR FROM data_fattura) AS anno_fatturazione, COUNT(*) AS numero_fatture, SUM(importo) AS somma_importi
FROM Fatture
GROUP BY EXTRACT(YEAR FROM data_fattura)
ORDER BY anno_fatturazione




Estrarre i prodotti attivati nel 2017 e che sono in produzione oppure in commercio



SELECT *
FROM Prodotti
WHERE EXTRACT(YEAR FROM data_attivazione) = 2017
  AND (in_produzione = true OR in_commercio = true)


Estrarre il numero di fatture per ogni anno considerando solo le fatture con iva al 20%


SELECT EXTRACT(YEAR FROM data_fattura) AS anno_fatturazione, COUNT(*) AS numero_fatture
FROM Fatture
WHERE iva = 20
GROUP BY EXTRACT(YEAR FROM data_fattura)
ORDER BY anno_fatturazione



Estrarre gli anni in cui sono state registrate più di due fatture con tipologia “saldo”


SELECT EXTRACT(YEAR FROM data_fattura) AS anno_fatturazione, COUNT(*) AS numero_fatture
FROM fatture
WHERE tipologia = 'saldo'
GROUP BY EXTRACT(YEAR FROM data_fattura)
HAVING COUNT(*) > 2



Riportare l elelnco delle fatture (numero, importo, iva e data) con in aggiunta il nome del fornitore




SELECT numero_fattura, importo, iva, data_fattura, denominazione AS nome_fornitore
FROM Fatture
JOIN Fornitori ON Fatture.numero_fornitore = Fornitori.numero_fornitore



Estrarre il totale degli importi delle fatture divisi per residenza dei clienti



SELECT clienti.residenza, SUM(fatture.importo) AS totale_importi
FROM fatture 
JOIN clienti  ON fatture.id_cliente = clienti.numero_cliente
GROUP BY clienti.residenza



Estrarre il numero dei clienti nati nel 1980 che hanno almeno una fattura superiore a 50 euro


SELECT COUNT(clienti.numero_cliente) AS numero_clienti
FROM Clienti 
JOIN Fatture  ON clienti.numero_cliente = fatture.id_cliente
WHERE EXTRACT(YEAR FROM clienti.data_nascita) = 1980
  AND fatture.importo > 50



Estrarre una colonna di nome “denominazione” contenente il nome , seguito da un carattere “-“, seguito dal cognome, per i soli clienti residenti nella regione lombardia



SELECT CONCAT(nome, '-', cognome) AS denominazione
FROM clienti
WHERE regione = 'lombardia'
