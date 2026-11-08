## Ejercicio 1
![[Pasted image 20261030000200.png|627]]

## Ejercicio 2
**![[Pasted image 20261030113202.png|615]]
/Esta captura es de cómo accedí al usuario 

SELECT DISTINCT patient_name, patient_id
FROM patient p
WHERE p.patient_id NOT IN (
	SELECT DISTINCT a.patient_id
	FROM appointment a
	WHERE a.contact_phone <> p.primary_phone 
		OR a.contact_phone = p.secondary_phone)
![[Pasted image 20261030163031.png|387]]

## Ejercicio 3
CREATE VIEW doctors_per_patients
SELECT p.patient_id, d.doctor_id
FROM PATIENT p 
INNER JOIN DOCTOR d ON p.patient_city = d.doctor_city;
![[Pasted image 20261030171523.png]]

## Ejercicio 4a
SELECT p.patient_id, COUNT(doctor_id) AS cantidad_doctores
FROM doctors_per_patients
GROUP BY patient_id;
![[Pasted image 20261030182028.png|533]]

## Ejercicio 4b
SELECT p.patient_name 
FROM PATIENT p 
WHERE p.patient_id NOT IN (
	SELECT patient_id
	FROM doctors_per_patients
);
![[Pasted image 20261030200833.png]]

## Ejercicio 4c
SELECT DISTINCT d1.doctor_id 
FROM doctors_per_patients d1
WHERE 5 <= (
	SELECT COUNT (d2.patient_id)
	FROM doctors_per_patients d2
	WHERE d2.doctor_id = d1.doctor_id
);
otra opción:
SELECT DISTINCT doctor_id
FROM doctors_per_patients
GROUP BY doctor_id
HAVING COUNT(patient_id) > 5;
![[Pasted image 20261030204301.png]]
![[Pasted image 20261030204313.png]]

## Ejercicio 5
CREATE TABLE APPOINTMENTS_PER_PATIENT (
	idApP INT(11) AUTO_INCREMENT PRIMARY KEY,
	patient_id INT(11),
	count_appointments INT(11),
	last_update DATETIME.
	user VARCHAR(16)
);
![[Pasted image 20261030210251.png]]

## Ejercicio 6
DELIMITER //
CREATE PROCEDURE sp_calcular_appointments ()
BEGIN
	DECLARE cursor_pacientes CURSOR FOR
		SELECT patient_id, COUNT(patient_id)
		FROM APPOINTMENT
		GROUP BY patient_id;
		
	DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
	DECLARE EXIT HANDLER FOR SQLEXCEPTION
	BEGIN
		ROLLBACK
	END;
	
	SET
END
DELIMITER ;