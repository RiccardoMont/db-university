
1. Selezionare tutti gli studenti nati nel 1990 (160)
SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`) = 1990;

/-------------------------------------------------------------/

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT * 
FROM `courses` 
WHERE `cfu` > 10;

/-------------------------------------------------------------/

3. Selezionare tutti gli studenti che hanno più di 30 anni
SELECT * 
FROM `students` 
WHERE (DATEDIFF(CURRENT_TIMESTAMP, `date_of_birth`)/365) >= 30; (3693 results)
Metodo dove si calcola l'anno anche in base alla differenza di mesi e giorni.


SELECT * 
FROM `students` 
WHERE (YEAR(CURRENT_TIMESTAMP) - YEAR(`date_of_birth`)) >= 30; (3799 results)
Metodo dove tengo conto solo dell'anno e, di conseguenza, meno stringente del primo metodo.

/-------------------------------------------------------------/

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
SELECT * 
FROM `courses` 
WHERE `year` = 1 
AND 
`period` = 'I semestre';

/-------------------------------------------------------------/

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
SELECT * 
FROM `exams` 
WHERE `date` = '2020-06-20' 
AND `hour` 
BETWEEN '13:59:59' 
AND '23:59:59';

/-------------------------------------------------------------/

6. Selezionare tutti i corsi di laurea magistrale (38)
SELECT * FROM `degrees` WHERE `level` = 'magistrale';

/-------------------------------------------------------------/

7. Da quanti dipartimenti è composta l'università? (12)
SELECT COUNT(`id`) FROM `departments`;

/-------------------------------------------------------------/

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT * FROM `teachers` WHERE `phone` IS NULL;