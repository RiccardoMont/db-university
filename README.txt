
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
WHERE (DATEDIFF(CURRENT_TIMESTAMP, `date_of_birth`)/365.25) >= 30; (3693 results)
Metodo dove si calcola l'anno anche in base alla differenza di mesi e giorni.


SELECT * 
FROM `students` 
WHERE (YEAR(CURRENT_TIMESTAMP) - YEAR(`date_of_birth`)) >= 30; (3799 results)
Metodo dove tengo conto solo dell'anno e, di conseguenza, meno stringente del primo metodo.


SELECT * 
FROM `students` 
WHERE TIMESTAMPDIFF(YEAR, dateofbirth, CURDATE()) > 30;
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





/------------------------------------------------------------------/



GROUP BY:
1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(`students`.`enrolment_date`) AS `data_entrata`, YEAR(`students`.`enrolment_date`) AS `anno` 
FROM `students`
GROUP BY YEAR(`students`.`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(`teachers`.`office_address`) AS `teacher_no`, `teachers`.`office_address` AS `indirizzo` 
FROM `teachers`
GROUP BY `teachers`.`office_address`;


3. Calcolare la media dei voti di ogni appello d'esame
SELECT  `exam_student`.`exam_id` AS `exam_id`, AVG(`exam_student`.`vote`) AS `average_vote`
FROM `exam_student`
GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `degrees`.`department_id` AS `department_no`, COUNT(`degrees`.`name`) AS `number_of_degree`
FROM `degrees`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
GROUP BY `degrees`.`department_id`;


/---------------------------------/

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT `students`.`name`, `students`.`surname`, `degrees`.`name` AS `degree_name`
FROM `students`
JOIN `degrees`
ON `students` . `degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
SELECT `degrees`.`name`, `degrees`.`level`, `departments`.`name` AS `departments_name`
FROM `degrees`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id` 
WHERE `degrees`.`level` = 'magistrale'
AND `departments`.`name` = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`
FROM `courses`
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers` ON `course_teacher`.`teacher_id` =  `teachers`.`id`
WHERE `teachers`.`id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
SELECT `students`.`surname`, `students`.`name` AS `name_student`,  `degrees`.`name` AS `degree_name`, `departments`.`name` AS `departments_name`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname`, `students`.`name`;


5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT `degrees`.`name` AS `degree_name`, `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
SELECT `departments`.`name` AS `department_name`, `degrees`.`name` AS `degree_name`, `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname` 
FROM `teachers`
JOIN `course_teacher` ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.


