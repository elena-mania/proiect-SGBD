CREATE TABLE adresa (
    id_adresa    NUMBER(10) PRIMARY KEY,
    tara         VARCHAR(50),
    oras         VARCHAR(50),
    strada       VARCHAR(50),
    numar        VARCHAR(5),
    cod_postal   VARCHAR(10)
);

CREATE TABLE utilizator (
    id_utilizator       NUMBER(5) PRIMARY KEY,
    nume                VARCHAR(50),
    prenume             VARCHAR(50),
    telefon             VARCHAR(10)
        CONSTRAINT verificare_telefon CHECK ( telefon LIKE '07________' ),
    data_inregistrare   DATE,
    id_adresa           NUMBER(10),
    FOREIGN KEY ( id_adresa )
        REFERENCES adresa ( id_adresa )
);

CREATE TABLE tranzactie (
    id_tranzactie     NUMBER(5) PRIMARY KEY,
    id_adresa         NUMBER(10),
    FOREIGN KEY ( id_adresa )
        REFERENCES adresa ( id_adresa ),
    id_vanzator       NUMBER(5),
    FOREIGN KEY ( id_vanzator )
        REFERENCES utilizator ( id_utilizator ),
    id_cumparator     NUMBER(5),
    FOREIGN KEY ( id_cumparator )
        REFERENCES utilizator ( id_utilizator ),
    CONSTRAINT check_different_ids CHECK ( id_vanzator <> id_cumparator ),
    status_plata      VARCHAR(50),
    status_comanda    VARCHAR(50),
    metoda_plata      VARCHAR(50),
    data_efectuarii   DATE
);

CREATE TABLE produs (
    id_produs       NUMBER(5) PRIMARY KEY,
    pret            INT,
    marime          VARCHAR(10),
    culoare         VARCHAR(50),
    firma           VARCHAR(50),
    calitate        VARCHAR(50),
    material        VARCHAR(50),
    sex             VARCHAR(50) CHECK ( sex IN (
        'masculin',
        'feminin',
        'unisex'
    ) ),
    id_utilizator   NUMBER(5),
    id_tranzactie   NUMBER(5) NULL,
    FOREIGN KEY ( id_utilizator )
        REFERENCES utilizator ( id_utilizator ),
    FOREIGN KEY ( id_tranzactie )
        REFERENCES tranzactie ( id_tranzactie )
);

CREATE TABLE categorie (
    id_categorie   INT PRIMARY KEY,
    denumire       VARCHAR(50)
);

CREATE TABLE recenzie (
    id_recenzie                        NUMBER(5) PRIMARY KEY,
    evaluare_produs                    VARCHAR(50),
    evaluare_relationare_utilizatori   VARCHAR(100),
    rating                             NUMBER(5) CHECK ( rating >= 1
                             AND rating <= 5 ),
    id_utilizator                      NUMBER(5),
    id_tranzactie                      NUMBER(5),
    FOREIGN KEY ( id_utilizator )
        REFERENCES utilizator ( id_utilizator ),
    FOREIGN KEY ( id_tranzactie )
        REFERENCES tranzactie ( id_tranzactie )
);

CREATE TABLE LISTA_FAVORITE(
    id_lista NUMBER(5) PRIMARY KEY,
    id_utilizator NUMBER(5),
    status VARCHAR(10) CHECK ( status IN ('arhivat','sters','activ')),
    data_creare DATE,
    FOREIGN KEY (ID_UTILIZATOR) REFERENCES UTILIZATOR(id_utilizator)
);

CREATE TABLE categorie_produs (
    id_categorie   INT,
    id_produs      NUMBER(5),
    PRIMARY KEY ( id_categorie,
                  id_produs ),
    FOREIGN KEY ( id_categorie )
        REFERENCES categorie ( id_categorie ),
    FOREIGN KEY ( id_produs )
        REFERENCES produs ( id_produs )
);

CREATE TABLE lista_favorite_produs (
    id_lista    NUMBER(5),
    id_produs   NUMBER(5),
    PRIMARY KEY ( id_lista,
                  id_produs ),
    FOREIGN KEY ( id_lista )
        REFERENCES lista_favorite ( id_lista ),
    FOREIGN KEY ( id_produs )
        REFERENCES produs ( id_produs )
);

--Inserarea in tabela ADRESA:
INSERT INTO ADRESA VALUES (10,'Romania','Roman','Stefan cel Mare','26','611038');
INSERT INTO ADRESA VALUES (11,'Romania','Bucuresti','Pictor Arthur Verona','12','010314');
INSERT INTO ADRESA VALUES (12,'Romania','Bucuresti','Fizicienilor','6','032064');
INSERT INTO ADRESA VALUES (13,'Romania','Bucuresti','Masina de Paine','37','021125');
INSERT INTO ADRESA VALUES (14,'Romania','Bucuresti','Lirei','1','021421');
INSERT INTO ADRESA VALUES (15,'Romania','Braila','Pacurari','54','700524');
INSERT INTO ADRESA VALUES (16,'Romania','Husi','Schit','3','735100');
INSERT INTO ADRESA VALUES (17,'Romania','Oradea','Horea','10','410080');
INSERT INTO ADRESA VALUES (18,'Romania','Constanta','Decebal','15','900669');
INSERT INTO ADRESA VALUES (19,'Romania','Timisoara','Magura','4','300763');
INSERT INTO ADRESA VALUES (20,'Romania','Sibiu','Independentei','1','550173');
INSERT INTO ADRESA VALUES (21,'Romania','Iasi','Tanasescu','33','700030');
INSERT INTO ADRESA VALUES (22,'Romania','Baia Mare','Berzei','79','430333');
INSERT INTO ADRESA VALUES (23,'Romania','Cluj-Napoca','Gheorghe Lazar','22','400183');
INSERT INTO ADRESA VALUES (24,'Romania','Iasi','Bujorilor','24','2009');
INSERT INTO ADRESA VALUES (25,'Romania','Bistrita','Cramei','8','2005');
INSERT INTO ADRESA VALUES (26,'Romania','Arad','Lenin','17','3300');

--Inserari in tabela UTILIZATOR:
INSERT INTO UTILIZATOR VALUES (10006, 'Popescu','Andreea','0712345678',TO_DATE('2023-06-01','YYYY-MM-DD'),11);
INSERT INTO UTILIZATOR VALUES (10002, 'Andreescu','Ion','0712346678',TO_DATE('2021-10-12','YYYY-MM-DD'),12);
INSERT INTO UTILIZATOR VALUES (10003, 'Calinescu','Marina','0745645678',TO_DATE('2022-03-12','YYYY-MM-DD'),15);
INSERT INTO UTILIZATOR VALUES (10001, 'Popescu','Andreea','0716377778', TO_DATE('2020-05-15','YYYY-MM-DD'),22);
INSERT INTO UTILIZATOR VALUES (10004, 'Vasilache','Cristiana','0794365678', TO_DATE('2022-06-11','YYYY-MM-DD'),18);
INSERT INTO UTILIZATOR VALUES (10007, 'Copaceanu','Mihai','0744045078',TO_DATE('2021-09-24','YYYY-MM-DD'),23);
INSERT INTO UTILIZATOR VALUES (10008, 'Iliescu', 'Maria','0742066079', TO_DATE('2023-12-04','YYYY-MM-DD'),10);
INSERT INTO UTILIZATOR VALUES (10005, 'Marinescu','Iulian','0745045076', TO_DATE('2023-03-24','YYYY-MM-DD'),16);
INSERT INTO UTILIZATOR VALUES (10009, 'Romascu', 'Sergiu', '0767380297', TO_DATE('2023-06-25','YYYY-MM-DD'),13);
INSERT INTO UTILIZATOR VALUES (10010, 'Gosav','Doina','0765432780',TO_DATE('2023-06-25','YYYY-MM-DD'),14);
INSERT INTO UTILIZATOR VALUES (10011, 'Oprea','Alexandru','0794365321',TO_DATE('2021-09-20','YYYY-MM-DD'),17);
INSERT INTO UTILIZATOR VALUES (10012, 'Caliniuc', 'Ramona','0712040608',TO_DATE('2021-10-12','YYYY-MM-DD'),19);
INSERT INTO UTILIZATOR VALUES (10013, 'Racovita','Florin','0744045456',TO_DATE('2021-10-12','YYYY-MM-DD'),20);
INSERT INTO UTILIZATOR VALUES (10014, 'Florea', 'Catalina', '0742066079',TO_DATE('2020-5-12','YYYY-MM-DD'), 21);
INSERT INTO UTILIZATOR VALUES (10015, 'Onciu','Gregorian','0716370678',TO_DATE('2023-12-21','YYYY-MM-DD'),24);
INSERT INTO UTILIZATOR VALUES (10016, 'Lazarescu', 'Ana-Maria','0765432781',TO_DATE('2020-10-10','YYYY-MM-DD'),25);
INSERT INTO UTILIZATOR VALUES (10017, 'Apostol','Luca','0732146678',TO_DATE('2023-12-21','YYYY-MM-DD'),26);

--Inserarea in tabela CATEGORIE
INSERT INTO CATEGORIE VALUES (1,'Imbracaminte exterioara');
INSERT INTO CATEGORIE VALUES (2,'Palton');
INSERT INTO CATEGORIE VALUES (3,'Geaca');
INSERT INTO CATEGORIE VALUES (4,'Pelerina');
INSERT INTO CATEGORIE VALUES (5,'Jacheta');
INSERT INTO CATEGORIE VALUES (6,'Pulover');
INSERT INTO CATEGORIE VALUES (7,'Hanorac');
INSERT INTO CATEGORIE VALUES (8,'Bluze');
INSERT INTO CATEGORIE VALUES (9,'Tricou');
INSERT INTO CATEGORIE VALUES (10,'Maleta');
INSERT INTO CATEGORIE VALUES (11,'Camasa');
INSERT INTO CATEGORIE VALUES (12,'Sacou');
INSERT INTO CATEGORIE VALUES (13,'Fusta');
INSERT INTO CATEGORIE VALUES (14,'Rochie');
INSERT INTO CATEGORIE VALUES (15,'Pantaloni');
INSERT INTO CATEGORIE VALUES (16,'Incaltaminte');
INSERT INTO CATEGORIE VALUES (17,'Imbracaminte Sport');
INSERT INTO CATEGORIE VALUES (18,'Imbracaminte Formala');
INSERT INTO CATEGORIE VALUES (19,'Imbracaminte Casual');
INSERT INTO CATEGORIE VALUES (20,'Tocuri');
INSERT INTO CATEGORIE VALUES (21,'Blugi');
INSERT INTO CATEGORIE VALUES (22,'Ghete');
INSERT INTO CATEGORIE VALUES (23,'Adidasi');

--Inserare in tabela PRODUS:
INSERT INTO PRODUS VALUES (1,50,'Mediu','Bej','H/M','Purtat','Bumbac','feminin', 10003,12345);
INSERT INTO PRODUS VALUES (2,500,'Small','Alb','Ralph-Lauren','Uzat','Tricot','feminin', 10003,12345);
INSERT INTO PRODUS VALUES (3,45,'Large','Negru','H/M','Nepurtat','Stofa','masculin', 10003,12347);
INSERT INTO PRODUS VALUES (4,120,'Large','Roz','Ralph-Lauren','Nepurtat','Poliester','feminin', 10003, NULL);
INSERT INTO PRODUS VALUES (5,250,'45','Alb','Nike','Nepurtat','Panza','masculin',10007,12349);
INSERT INTO PRODUS VALUES (6,37,'Large','Albastru deschis','Bershka','Purtat','Poliester','masculin', 10002, NULL);
INSERT INTO PRODUS VALUES (7,44,'Small','Multicolor','NoName','Nepurtat','Bumbac','unisex', 10001,12350);
INSERT INTO PRODUS VALUES (8,50,'Mediu','Bej','H/M','Purtat','Bumbac','feminin', 10003,12348);
INSERT INTO PRODUS VALUES (9,13,'Large','Alb','NoName','Uzat','Tricot','feminin', 10009, NULL);
INSERT INTO PRODUS VALUES (10,45,'36','Negru','H/M','Nepurtat','Stofa','masculin', 10007, 12351);
INSERT INTO PRODUS VALUES (11,5,'Large','Grena','NoName','Nepurtat','Poliester','feminin', 10015,NULL);
INSERT INTO PRODUS VALUES (12,10,'39','Alb','Vans','Uzat','Panza','unisex', 10015,12350);
INSERT INTO PRODUS VALUES (13,22,'Large','Albastru inchis','Bershka','Purtat','Poliester','masculin', 10015, NULL);
INSERT INTO PRODUS VALUES (14,44,'Small','Multicolor','NoName','Uzat','Bumbac','unisex', 10017, 12346);
INSERT INTO PRODUS VALUES (15,46,'Small','Albastru','Zara','Nou cu eticheta','Bumbac','unisex', 10017, NULL);
INSERT INTO PRODUS VALUES (16,47,'34','Roz','H/M','Nepurtat','Bumbac','unisex', 10017, NULL);


--Inserare in tabela TRANZACTIE:
INSERT INTO TRANZACTIE VALUES (12345,18,10003,10004,'Platit', 'Livrat', 'Card',TO_DATE('2022-11-20','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12346,16,10017,10005, 'Neplatit', 'Expediat', 'Ramburs',TO_DATE('2023-12-30','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12347,18,10003,10004,'Platit', 'Livrat', 'Card',TO_DATE('2022-07-20','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12348,12,10003,10002, 'Platit', 'Livrat', 'Card',TO_DATE('2023-06-17','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12349,20,10007,10013, 'Neplatit', 'Neprocesat', 'Card',TO_DATE('2023-06-22','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12350,25,10001,10016, 'Platit', 'Livrat', 'Card', TO_DATE('2022-11-25','YYYY-MM-DD'));
INSERT INTO TRANZACTIE VALUES (12351,14,10007,10010, 'Platit', 'Livrat', 'Ramburs', TO_DATE('2023-6-12','YYYY-MM-DD'));

--Inserare in tabela RECENZIE
INSERT INTO RECENZIE VALUES(10101,'Excelent','Comunicare promta', 5, 10004, 12345 );
INSERT INTO RECENZIE VALUES(10102,NULL ,'Relationare buna', 5, 10003, 12345);
INSERT INTO RECENZIE VALUES(10103,'Calitate inalta', NULL , 4, 10002, 12348);
INSERT INTO RECENZIE VALUES(10104,NULL, 'Comunicare defectuoasa', 2, 10001, 12350);
INSERT INTO RECENZIE VALUES(10105,'Produs slab calitativ', 'Cumparator serios',3, 10010, 12351);
INSERT INTO RECENZIE VALUES(10106,NULL, 'Neintelegeri', 1, 10016, 12350);

--Inserare in tabela LISTA_FAVORITE
INSERT INTO LISTA_FAVORITE VALUES(1,10004,'activ',TO_DATE('2022-07-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(2,10002,'arhivat',TO_DATE('2021-11-22','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(3,10006,'activ',TO_DATE('2023-11-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(4,10013,'activ',TO_DATE('2021-11-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(5,10016,'sters',TO_DATE('2020-10-25','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(6,10005,'arhivat',TO_DATE('2022-11-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(7,10010,'arhivat',TO_DATE('2023-03-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(8,10017,'sters',TO_DATE('2023-12-21','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(9,10003,'activ',TO_DATE('2022-03-12','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(10,10001,'arhivat',TO_DATE('2020-11-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(11,10007,'activ',TO_DATE('2021-09-30','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(12,10008,'arhivat',TO_DATE('2023-12-04','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(13,10009,'sters',TO_DATE('2021-09-30','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(14,10011,'arhivat',TO_DATE('2021-09-20','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(15,10012,'activ',TO_DATE('2021-09-30','YYYY-MM-DD'));
INSERT INTO LISTA_FAVORITE VALUES(16,10014,'sters',TO_DATE('2021-11-20','YYYY-MM-DD'));

--Inserarea in tabela CATEGORIE_PRODUS 
INSERT INTO CATEGORIE_PRODUS VALUES (11,1);
INSERT INTO CATEGORIE_PRODUS VALUES (19,1);
INSERT INTO CATEGORIE_PRODUS VALUES (9,2);
INSERT INTO CATEGORIE_PRODUS VALUES (12,3);
INSERT INTO CATEGORIE_PRODUS VALUES (18,3);
INSERT INTO CATEGORIE_PRODUS VALUES (14,4);
INSERT INTO CATEGORIE_PRODUS VALUES (18,4);
INSERT INTO CATEGORIE_PRODUS VALUES (16,5);
INSERT INTO CATEGORIE_PRODUS VALUES (23,5);
INSERT INTO CATEGORIE_PRODUS VALUES (17,5);
INSERT INTO CATEGORIE_PRODUS VALUES (1,6);
INSERT INTO CATEGORIE_PRODUS VALUES (4,6);
INSERT INTO CATEGORIE_PRODUS VALUES (9,7);
INSERT INTO CATEGORIE_PRODUS VALUES (5,8);
INSERT INTO CATEGORIE_PRODUS VALUES (1,8);
INSERT INTO CATEGORIE_PRODUS VALUES (19,8);
INSERT INTO CATEGORIE_PRODUS VALUES (10,9);
INSERT INTO CATEGORIE_PRODUS VALUES (15,10);
INSERT INTO CATEGORIE_PRODUS VALUES (18,10);
INSERT INTO CATEGORIE_PRODUS VALUES (15,11);
INSERT INTO CATEGORIE_PRODUS VALUES (16,12);
INSERT INTO CATEGORIE_PRODUS VALUES (18,12);
INSERT INTO CATEGORIE_PRODUS VALUES (3,13);
INSERT INTO CATEGORIE_PRODUS VALUES (9,14);
INSERT INTO CATEGORIE_PRODUS VALUES (12,15);
INSERT INTO CATEGORIE_PRODUS VALUES (19,15);
INSERT INTO CATEGORIE_PRODUS VALUES (16,16);
INSERT INTO CATEGORIE_PRODUS VALUES (18,16);

--Inserarea in tabela LISTA_FAVORITE_PRODUS
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (1,1);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (1,2);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (1,3);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (1,4);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (2,1);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (3,5);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (3,9);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (4,6);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (5,15);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (5,16);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (6,9);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (7,6);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (8,1);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (9,1);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (10,12);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (10,11);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (10,13);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (11,12);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (12,9);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (12,14);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (12,15);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (12,16);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (13,8);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (14,9);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (14,10);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (15,12);
INSERT INTO LISTA_FAVORITE_PRODUS VALUES (16,3);

commit;

--EXERCITIUL 6
CREATE OR REPLACE PROCEDURE exercitiul_6 IS
    TYPE tabel_produse IS TABLE OF VARCHAR2(50);   
    TYPE coduri_utilizator IS VARRAY(50) OF utilizator.id_utilizator%TYPE;
    TYPE tabel_pret_produse IS TABLE OF NUMBER INDEX BY PLS_INTEGER;
    
    produse_nevandute tabel_produse := tabel_produse();
    cod_utilizatori coduri_utilizator := coduri_utilizator();
    suma_produse_fav tabel_pret_produse;
    utilizator_fara_vanzari utilizator.id_utilizator%TYPE;
    ct int := 1;
BEGIN
    SELECT id_utilizator INTO utilizator_fara_vanzari
    FROM (
        SELECT id_utilizator, COUNT(id_produs) AS numar_produse
        FROM produs
        WHERE id_tranzactie IS NULL
        GROUP BY id_utilizator
        ORDER BY numar_produse DESC
    )
    WHERE ROWNUM = 1;
    
    FOR cod IN (SELECT l.id_utilizator
                FROM lista_favorite l
                JOIN utilizator u ON l.id_utilizator = u.id_utilizator
                WHERE l.data_creare = u.data_inregistrare) LOOP
        cod_utilizatori.EXTEND;
        cod_utilizatori(ct) := cod.id_utilizator;
        ct := ct + 1;
    END LOOP;

    --stiu ca cod_utilizatori si suma+produse_fav au acelasi nr de elem,  nu mai am nev de ct
    FOR i IN cod_utilizatori.FIRST..cod_utilizatori.LAST LOOP
        SELECT SUM(p.pret) INTO suma_produse_fav(i)
        FROM produs p
        JOIN lista_favorite_produs lf ON lf.id_produs = p.id_produs
        JOIN lista_favorite l ON lf.id_lista = l.id_lista
        WHERE l.id_utilizator = cod_utilizatori(i);
    END LOOP;
    FOR i IN suma_produse_fav.FIRST..suma_produse_fav.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Suma preturilor pentru Id-ul '|| COD_UTILIZATORI(i) || ' este ' || suma_produse_fav(i));
    END LOOP;

    FOR produs IN (SELECT firma, marime, culoare, calitate, sex 
                   FROM produs 
                   WHERE id_utilizator = utilizator_fara_vanzari) LOOP
        produse_nevandute.EXTEND;
        produse_nevandute(produse_nevandute.LAST) := produs.firma || ', ' || produs.marime || ', ' ||
                                                     produs.culoare || ', ' || produs.calitate || ', ' ||
                                                     produs.sex;
    END LOOP;

    FOR i IN produse_nevandute.FIRST..produse_nevandute.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Informatii produs: ');
        DBMS_OUTPUT.PUT_LINE(produse_nevandute(i));
    END LOOP;
    
END exercitiul_6;
/

BEGIN
    exercitiul_6;
END;
/

--EXERCITIUL 7
CREATE OR REPLACE PROCEDURE Exercitiul_7 IS
    CURSOR c_utilizatori_cu_recenzii IS
        SELECT DISTINCT u.id_utilizator, u.nume, u.prenume
        FROM utilizator u
        JOIN tranzactie t ON u.id_utilizator = t.id_cumparator
        JOIN recenzie r ON t.id_tranzactie = r.id_tranzactie;
    CURSOR c_recenzii (p_id_utilizator NUMBER) IS
        SELECT r.evaluare_produs, r.evaluare_relationare_utilizatori, r.rating
        FROM recenzie r
        JOIN tranzactie t ON r.id_tranzactie = t.id_tranzactie
        WHERE t.id_cumparator = p_id_utilizator AND r.id_utilizator = p_id_utilizator;
    v_utilizator c_utilizatori_cu_recenzii%ROWTYPE;
    v_recenzie c_recenzii%ROWTYPE;
BEGIN
    OPEN c_utilizatori_cu_recenzii;
    LOOP
        FETCH c_utilizatori_cu_recenzii INTO v_utilizator;
        EXIT WHEN c_utilizatori_cu_recenzii%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('Utilizatorul: ' || v_utilizator.nume || ' ' || v_utilizator.prenume);

        -- Deschiderea cursorului parametrizat pentru recenzii
        OPEN c_recenzii(v_utilizator.id_utilizator);
        LOOP
            FETCH c_recenzii INTO v_recenzie;
            EXIT WHEN c_recenzii%NOTFOUND;
            DBMS_OUTPUT.PUT_LINE('   Recenzie: Evaluare Produs - ' || v_recenzie.evaluare_produs ||
                                 '; Evaluare Relationare - ' || v_recenzie.evaluare_relationare_utilizatori ||
                                 '; Rating - ' || v_recenzie.rating);
        END LOOP;
        CLOSE c_recenzii;
    END LOOP;
    CLOSE c_utilizatori_cu_recenzii;
END Exercitiul_7;

/
BEGIN
Exercitiul_7;
end;
/

--EXERCITIUL 8
CREATE OR REPLACE FUNCTION Exercitiul_8(p_data_inceput DATE, p_data_sfarsit DATE)
RETURN VARCHAR2 IS
    denumire_categorie VARCHAR2(50);
    max_vanzari INT;
    data_invalida EXCEPTION;
    interval_prea_mare EXCEPTION;

BEGIN
    IF p_data_inceput > p_data_sfarsit THEN
        RAISE data_invalida;
    END IF;

    IF p_data_sfarsit - p_data_inceput > 365 THEN
        RAISE interval_prea_mare;
    END IF;

    SELECT c.denumire INTO denumire_categorie
    FROM categorie c
    JOIN categorie_produs cp ON c.id_categorie = cp.id_categorie
    JOIN produs p ON cp.id_produs = p.id_produs
    JOIN tranzactie t ON p.id_tranzactie = t.id_tranzactie
    WHERE t.data_efectuarii BETWEEN p_data_inceput AND p_data_sfarsit
    GROUP BY c.denumire
    ORDER BY COUNT(*) DESC, c.denumire
    FETCH FIRST 1 ROW ONLY;

    RETURN denumire_categorie;
EXCEPTION
    WHEN data_invalida THEN
        DBMS_OUTPUT.PUT_LINE('Eroare: Data de inceput este mai mare decât data de sfarsit.');
        RETURN NULL;
    WHEN interval_prea_mare THEN
        DBMS_OUTPUT.PUT_LINE('Eroare: Intervalul de timp specificat este prea mare.');
        RETURN NULL;
END Exercitiul_8;
/
DECLARE
    test1 VARCHAR2(50);
    test2 VARCHAR2(50);
    test3 VARCHAR2(50);
BEGIN
    --input-ul nu este corect
    test1:= Exercitiul_8(TO_DATE('2023-10-01', 'YYYY-MM-DD'), TO_DATE('2020-03-01', 'YYYY-MM-DD'));
    --diferenta de timp este prea mare
    test2:= Exercitiul_8(TO_DATE('2020-01-01', 'YYYY-MM-DD'), TO_DATE('2024-01-01', 'YYYY-MM-DD'));
    --inputul este corect
    test3:= Exercitiul_8(TO_DATE('2022-01-01', 'YYYY-MM-DD'), TO_DATE('2023-01-01', 'YYYY-MM-DD'));
    IF test3 IS NOT NULL THEN
        DBMS_OUTPUT.PUT_LINE('Categoria cu cele mai multe vanzari: ' || test3);
    END IF;
END;
/
--EXERCITIUL 9
CREATE OR REPLACE PROCEDURE Exercitiul_9(
    p_oras IN VARCHAR2
) IS
    nume_utilizator VARCHAR2(100);
    prenume_utilizator VARCHAR2(100);
    castig_total NUMBER;
    utilizator_fara_vanzari EXCEPTION;
    verif_oras INT;
    medie_recenzii NUMBER;
BEGIN
    -- verificam daca orasul este unic: daca codul nu e corect se duce pe eroarea too many rows
    SELECT id_adresa INTO verif_oras
    FROM adresa
    WHERE oras = p_oras;

    -- selectam utilizatorul care indeplineste conditiile
    SELECT u.nume, u.prenume, SUM(p.pret), AVG(r.rating)
    INTO nume_utilizator, prenume_utilizator, castig_total, medie_recenzii
    FROM utilizator u
    JOIN adresa a ON u.id_adresa = a.id_adresa
    JOIN tranzactie t ON u.id_utilizator = t.id_vanzator
    LEFT JOIN produs p ON t.id_tranzactie = p.id_tranzactie
    LEFT JOIN recenzie r ON t.id_tranzactie = r.id_tranzactie AND t.id_cumparator = r.id_utilizator
    WHERE a.oras = p_oras
    GROUP BY u.nume, u.prenume
    HAVING COUNT(p.id_produs) >= 2 AND AVG(r.rating)> 2
    FETCH FIRST 1 ROW ONLY;

    DBMS_OUTPUT.PUT_LINE('Nume: ' || nume_utilizator || ', Prenume: ' || prenume_utilizator || ', Castig total: ' || castig_total || ', Medie recenzii: ' || medie_recenzii);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        IF verif_oras IS NULL THEN
        DBMS_OUTPUT.PUT_LINE('Eroare: Nu exista orasul ' || p_oras || ' in baza de date.');
        ELSE DBMS_OUTPUT.PUT_LINE('Eroare: Nu exista utilizator in orasul ' || p_oras || ' care sa indeplineasca conditiile.');
        END IF;
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('Eroare: Exista mai multi utilizatori care locuiesc in orasul ' || p_oras || '.');
END Exercitiul_9;
/

BEGIN
    Exercitiul_9('Bucuresti');
    Exercitiul_9('Deva');
    Exercitiul_9('Constanta');
    Exercitiul_9('Braila');
END;
/
commit;
--EXERCITIUL 10
CREATE OR REPLACE TRIGGER Exercitiul_10
    AFTER INSERT OR DELETE OR UPDATE ON produs
DECLARE 
    total_produse NUMBER:=0;
BEGIN
    select Count(*) into total_produse
    from produs
    where pret>=50 and pret<=100;
    
    IF total_produse<>0 then
        DBMS_OUTPUT.PUT_LINE('Numarul de produse este: '||total_produse);
    ELSE 
        DBMS_OUTPUT.PUT_LINE('Nu exista produse cuprinse in intervalul respectiv de pret.');
    END IF;
END;
/
--adaugarea unui produs
INSERT INTO PRODUS VALUES (17,53,'Small','Crem','NoName','Nepurtat','Bumbac','unisex', 10017, NULL);
select *from produs;
--stergerea unui produs
DELETE FROM PRODUS
WHERE id_produs=17;

DROP TRIGGER Exercitiul_10;

--EXERCITIUL 11
CREATE OR REPLACE TRIGGER Exercitiul_11
FOR DELETE ON lista_favorite
COMPOUND TRIGGER

    total NUMBER :=0;
    numar_sters NUMBER :=0;
    numar_ramas NUMBER:=0;
    BEFORE STATEMENT IS
    BEGIN
        SELECT COUNT(*) INTO total
        FROM lista_favorite;
    END BEFORE STATEMENT;

    BEFORE EACH ROW IS
    BEGIN
        IF :OLD.status <> 'sters' THEN
            RAISE_APPLICATION_ERROR(-20001, 'Informatia nu poate fi stearsa');
        END IF;
        DELETE FROM lista_favorite_produs l
        WHERE l.id_lista = :OLD.id_lista;
        
    END BEFORE EACH ROW;
    AFTER STATEMENT IS
    BEGIN
        SELECT COUNT(*) INTO numar_ramas
        FROM lista_favorite;
        numar_sters:=total-numar_ramas;
        DBMS_OUTPUT.PUT_LINE('Numarul de liste sterse este: '|| numar_sters);
        
    END AFTER STATEMENT;
END Exercitiul_11;
/
--Nu poate sterge, elementul nu e marcat ca fiind sters
DELETE FROM lista_favorite
where id_lista=1;
--stergerea se efectueaza cu succes
DELETE FROM lista_favorite
where status='sters';

rollback;
DROP TRIGGER Exercitiul_11;

--EXERCITIUL 12
CREATE OR REPLACE TRIGGER Exercitiul_12
AFTER CREATE OR DROP ON SCHEMA
DECLARE
  nr_tabele NUMBER;
BEGIN
  SELECT COUNT(*)
  INTO nr_tabele
  FROM user_tables;

  IF nr_tabele > 10 THEN
    DBMS_OUTPUT.PUT_LINE('Atentie: Numarul total de tabele este peste 10.');
  ELSE 
    DBMS_OUTPUT.PUT_LINE('Numarul de tabele este: '|| nr_tabele);
  END IF;
END Exercitiul_12;
/
CREATE TABLE tabel_test(id INT);
CREATE TABLE tabel_test2(id INT);
DROP TABLE tabel_test;
DROP TABLE tabel_test2;
rollback;
drop trigger exercitiul_12;


--EXERCITIUL 13
CREATE OR REPLACE PACKAGE pachet_ex_13
    AS
    PROCEDURE Exercitiul_6;
    PROCEDURE Exercitiul_7;
    FUNCTION Exercitiul_8(p_data_inceput DATE, p_data_sfarsit DATE) RETURN VARCHAR2;
    PROCEDURE Exercitiul_9(p_oras IN VARCHAR2);
    
END pachet_ex_13;
/
CREATE OR REPLACE PACKAGE BODY pachet_ex_13 AS 
    
        --EX6 PACHET
        PROCEDURE exercitiul_6 IS
                TYPE tabel_produse IS TABLE OF VARCHAR2(50);   
                TYPE coduri_utilizator IS VARRAY(50) OF utilizator.id_utilizator%TYPE;
                TYPE tabel_pret_produse IS TABLE OF NUMBER INDEX BY PLS_INTEGER;
                
                produse_nevandute tabel_produse := tabel_produse();
                cod_utilizatori coduri_utilizator := coduri_utilizator();
                suma_produse_fav tabel_pret_produse;
                utilizator_fara_vanzari utilizator.id_utilizator%TYPE;
                ct int := 1;
    BEGIN
        SELECT id_utilizator INTO utilizator_fara_vanzari
        FROM (
            SELECT id_utilizator, COUNT(id_produs) AS numar_produse
            FROM produs
            WHERE id_tranzactie IS NULL
            GROUP BY id_utilizator
            ORDER BY numar_produse DESC
        )
        WHERE ROWNUM = 1;
        
        FOR cod IN (SELECT l.id_utilizator
                    FROM lista_favorite l
                    JOIN utilizator u ON l.id_utilizator = u.id_utilizator
                    WHERE l.data_creare = u.data_inregistrare) LOOP
            cod_utilizatori.EXTEND;
            cod_utilizatori(ct) := cod.id_utilizator;
            ct := ct + 1;
        END LOOP;
    
        --stiu ca cod_utilizatori si suma+produse_fav au acelasi nr de elem,  nu mai am nev de ct
        FOR i IN cod_utilizatori.FIRST..cod_utilizatori.LAST LOOP
            SELECT SUM(p.pret) INTO suma_produse_fav(i)
            FROM produs p
            JOIN lista_favorite_produs lf ON lf.id_produs = p.id_produs
            JOIN lista_favorite l ON lf.id_lista = l.id_lista
            WHERE l.id_utilizator = cod_utilizatori(i);
        END LOOP;
        FOR i IN suma_produse_fav.FIRST..suma_produse_fav.LAST LOOP
            DBMS_OUTPUT.PUT_LINE('Suma preturilor pentru Id-ul '|| COD_UTILIZATORI(i) || ' este ' || suma_produse_fav(i));
        END LOOP;
    
        FOR produs IN (SELECT firma, marime, culoare, calitate, sex 
                       FROM produs 
                       WHERE id_utilizator = utilizator_fara_vanzari) LOOP
            produse_nevandute.EXTEND;
            produse_nevandute(produse_nevandute.LAST) := produs.firma || ', ' || produs.marime || ', ' ||
                                                         produs.culoare || ', ' || produs.calitate || ', ' ||
                                                         produs.sex;
        END LOOP;
    
        FOR i IN produse_nevandute.FIRST..produse_nevandute.LAST LOOP
            DBMS_OUTPUT.PUT_LINE('Informatii produs: ');
            DBMS_OUTPUT.PUT_LINE(produse_nevandute(i));
        END LOOP;
        
    END exercitiul_6;
    
    
    --EX 7 PACHET
    
    PROCEDURE Exercitiul_7 IS
        CURSOR c_utilizatori_cu_recenzii IS
            SELECT DISTINCT u.id_utilizator, u.nume, u.prenume
            FROM utilizator u
            JOIN tranzactie t ON u.id_utilizator = t.id_cumparator
            JOIN recenzie r ON t.id_tranzactie = r.id_tranzactie;
        CURSOR c_recenzii (p_id_utilizator NUMBER) IS
            SELECT r.evaluare_produs, r.evaluare_relationare_utilizatori, r.rating
            FROM recenzie r
            JOIN tranzactie t ON r.id_tranzactie = t.id_tranzactie
            WHERE t.id_cumparator = p_id_utilizator AND r.id_utilizator = p_id_utilizator;
        v_utilizator c_utilizatori_cu_recenzii%ROWTYPE;
        v_recenzie c_recenzii%ROWTYPE;
    BEGIN
        OPEN c_utilizatori_cu_recenzii;
        LOOP
            FETCH c_utilizatori_cu_recenzii INTO v_utilizator;
            EXIT WHEN c_utilizatori_cu_recenzii%NOTFOUND;
            DBMS_OUTPUT.PUT_LINE('Utilizatorul: ' || v_utilizator.nume || ' ' || v_utilizator.prenume);
    
            -- Deschiderea cursorului parametrizat pentru recenzii
            OPEN c_recenzii(v_utilizator.id_utilizator);
            LOOP
                FETCH c_recenzii INTO v_recenzie;
                EXIT WHEN c_recenzii%NOTFOUND;
                DBMS_OUTPUT.PUT_LINE('   Recenzie: Evaluare Produs - ' || v_recenzie.evaluare_produs ||
                                     '; Evaluare Relationare - ' || v_recenzie.evaluare_relationare_utilizatori ||
                                     '; Rating - ' || v_recenzie.rating);
            END LOOP;
            CLOSE c_recenzii;
        END LOOP;
        CLOSE c_utilizatori_cu_recenzii;
    END Exercitiul_7;
    
    
    --EX8 PACHET
    FUNCTION Exercitiul_8(p_data_inceput DATE, p_data_sfarsit DATE)
        RETURN VARCHAR2 IS
            denumire_categorie VARCHAR2(50);
            max_vanzari INT;
            data_invalida EXCEPTION;
            interval_prea_mare EXCEPTION;
        
        BEGIN
            IF p_data_inceput > p_data_sfarsit THEN
                RAISE data_invalida;
            END IF;
        
            IF p_data_sfarsit - p_data_inceput > 365 THEN
                RAISE interval_prea_mare;
            END IF;
        
            SELECT c.denumire INTO denumire_categorie
            FROM categorie c
            JOIN categorie_produs cp ON c.id_categorie = cp.id_categorie
            JOIN produs p ON cp.id_produs = p.id_produs
            JOIN tranzactie t ON p.id_tranzactie = t.id_tranzactie
            WHERE t.data_efectuarii BETWEEN p_data_inceput AND p_data_sfarsit
            GROUP BY c.denumire
            ORDER BY COUNT(*) DESC, c.denumire
            FETCH FIRST 1 ROW ONLY;
        
            RETURN denumire_categorie;
        EXCEPTION
            WHEN data_invalida THEN
                DBMS_OUTPUT.PUT_LINE('Eroare: Data de inceput este mai mare decât data de sfarsit.');
                RETURN NULL;
            WHEN interval_prea_mare THEN
                DBMS_OUTPUT.PUT_LINE('Eroare: Intervalul de timp specificat este prea mare.');
                RETURN NULL;
        END Exercitiul_8;
       
      --EX 9 PACHET
        PROCEDURE Exercitiul_9(
            p_oras IN VARCHAR2
        ) IS
            nume_utilizator VARCHAR2(100);
            prenume_utilizator VARCHAR2(100);
            castig_total NUMBER;
            utilizator_fara_vanzari EXCEPTION;
            verif_oras INT;
            medie_recenzii NUMBER;
        BEGIN
            -- verificam daca orasul este unic: daca codul nu e corect se duce pe eroarea too many rows
            SELECT id_adresa INTO verif_oras
            FROM adresa
            WHERE oras = p_oras;
        
            -- selectam utilizatorul care indeplineste conditiile
            SELECT u.nume, u.prenume, SUM(p.pret), AVG(r.rating)
            INTO nume_utilizator, prenume_utilizator, castig_total, medie_recenzii
            FROM utilizator u
            JOIN adresa a ON u.id_adresa = a.id_adresa
            JOIN tranzactie t ON u.id_utilizator = t.id_vanzator
            LEFT JOIN produs p ON t.id_tranzactie = p.id_tranzactie
            LEFT JOIN recenzie r ON t.id_tranzactie = r.id_tranzactie AND t.id_cumparator = r.id_utilizator
            WHERE a.oras = p_oras
            GROUP BY u.nume, u.prenume
            HAVING COUNT(p.id_produs) >= 2 AND AVG(r.rating)> 2
            FETCH FIRST 1 ROW ONLY;
        
            DBMS_OUTPUT.PUT_LINE('Nume: ' || nume_utilizator || ', Prenume: ' || prenume_utilizator || ', Castig total: ' || castig_total || ', Medie recenzii: ' || medie_recenzii);
        EXCEPTION
            WHEN NO_DATA_FOUND THEN
                IF verif_oras IS NULL THEN
                DBMS_OUTPUT.PUT_LINE('Eroare: Nu exista orasul ' || p_oras || ' in baza de date.');
                ELSE DBMS_OUTPUT.PUT_LINE('Eroare: Nu exista utilizator in orasul ' || p_oras || ' care sa indeplineasca conditiile.');
                END IF;
            WHEN TOO_MANY_ROWS THEN
                DBMS_OUTPUT.PUT_LINE('Eroare: Exista mai multi utilizatori care locuiesc in orasul ' || p_oras || '.');
        END Exercitiul_9;
END;
/

