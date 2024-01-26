# Hospital

This repository is for the Hospital database. Below is the schema of the database, the questions according to difficulty levels (easy, intermediate, hard) and my own answer to each question.

### Database schema

![schema](src/schema.png)

## Questions

### <u>Easy:</u>

1 Show first name, last name, and gender of patients whose gender is 'M'

```
SELECT
    first_name,
    last_name,
    gender
FROM patients
WHERE gender is "M";
```

2 Show first name and last name of patients who does not have allergies. (null)

```
SELECT
	first_name,
    last_name
FROM patients
WHERE allergies is NULL;
```

3 Show first name of patients that start with the letter 'C'

```
SELECT first_name
FROM patients
WHERE first_name like 'C%'
```

4 Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

```
SELECT
	first_name,
    last_name
FROM patients
WHERE weight between 100 and 120;
```

5 Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

```
UPDATE patients
SET allergies = 'NKA'
WHERE allergies is null;
```

6 Show first name and last name concatinated into one column to show their full name.

```
SELECT CONCAT(first_name, ' ', last_name) as full_name
FROM patients;
```

7 Show first name, last name, and the full province name of each patient.

Example: 'Ontario' instead of 'ON'

```
SELECT
  first_name,
  last_name,
  province_name
FROM patients as pa
JOIN province_names as pn
ON pa.province_id = pn.province_id;
```

8 Show how many patients have a birth_date with 2010 as the birth year.

```
SELECT count(*) as total_patients
FROM patients
WHERE YEAR(birth_date) = 2010;
```

9 Show the first_name, last_name, and height of the patient with the greatest height.

```
SELECT
  first_name,
  last_name,
  MAX(height) as height
FROM patients;
```

10 Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000

```
SELECT *
FROM patients
WHERE patient_id in (1,45,534,879,1000);
```

11 Show the total number of admissions

```
SELECT COUNT(*) as total_admissions
FROM admissions;
```

12 Show all the columns from admissions where the patient was admitted and discharged on the same day.

```
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

13 Show the patient id and the total number of admissions for patient_id 579.

```
SELECT
  patient_id,
  COUNT(*) as total_admissions
FROM admissions
WHERE patient_id = 579;
```

14 Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

```
SELECT distinct(city) as unique_city
FROM patients
WHERE province_id = 'NS';
```

15 Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70

```
SELECT
  first_name,
  last_name,
  birth_date
FROM patients
WHERE height > 160 AND weight > 70;
```

16 Write a query to find list of patients first_name, last_name, and allergies where allergies are not null and are from the city of 'Hamilton'

```
SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE
  allergies NOT NULL AND
  city = 'Hamilton';
```

---

### <u>Medium:</u>

1 Show unique birth years from patients and order them by ascending.

```
SELECT distinct(YEAR(birth_date))
FROM patients
ORDER BY birth_date asc;
```

2 Show unique first names from the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

```
SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(first_name) = 1;
```

3 Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```
SELECT
  patient_id,
  first_name
FROM patients
WHERE
  LEN(first_name) >= 6 AND
  first_name LIKE 's%s';
```

4 Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.

Primary diagnosis is stored in the admissions table.

```
SELECT
	p.patient_id,
    p.first_name,
    p.last_name
FROM patients as p
JOIN admissions a ON a.patient_id = p.patient_id
WHERE diagnosis = 'Dementia';
```

5 Display every patient's first_name. Order the list by the length of each name and then by alphabetically.

```
SELECT first_name
FROM patients
ORDER BY
  LEN(first_name),
  first_name ASC;
```

6 Show the total amount of male patients and the total amount of female patients in the patients table.

Display the two results in the same row.

```
SELECT
  SUM(Gender = 'M') as male_count,
  SUM(Gender = 'F') AS female_count
FROM patients;

```

7 Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'.

Show results ordered ascending by allergies then by first_name then by last_name.

```
SELECT
	first_name,
    last_name,
    allergies
FROM patients
WHERE allergies IN('Penicillin', 'Morphine')
ORDER BY
	allergies,
    first_name,
    last_name;
```

8 Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```
SELECT
  patient_id,
  diagnosis
FROM admissions
GROUP BY
	patient_id,
	diagnosis
HAVING COUNT(diagnosis) > 1;
```

9 Show the city and the total number of patients in the city.

Order from most to least patients and then by city name ascending.

```
SELECT
	city,
    COUNT(*) as total_patients_in_city
FROM patients
GROUP BY city
ORDER BY
	total_patients_in_city DESC,
    city ASC;
```

10 Show first name, last name and role of every person that is either patient or doctor.

The roles are either "Patient" or "Doctor"

```
SELECT first_name, last_name, 'Patient' AS role FROM patients
	UNION ALL
SELECT first_name, last_name, 'Doctor' AS role FROM doctors;
```

11 Show all allergies ordered by popularity. Remove NULL values from query.

```
SELECT allergies, COUNT(allergies) as total_diagnosis
FROM patients
WHERE allergies IS NOT NULL
GROUP BY allergies
ORDER BY total_diagnosis DESC;
```

12 Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

```
SELECT
	first_name,
    last_name,
    birth_date
FROM patients
WHERE YEAR(birth_date) BETWEEN 1970 AND 1979
ORDER BY birth_date ASC;
```

13 We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order

EX: SMITH,jane

```
SELECT CONCAT(upper(last_name), ',', lower(first_name)) AS full_name
FROM patients
ORDER BY first_name DESC;
```

14 Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

```
SELECT
	province_id,
    SUM(height) AS sum_height
FROM patients
GROUP BY province_id
HAVING sum_height >= 7000
ORDER BY sum_height ASC;
```

15 Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'

```
SELECT (MAX(weight) - MIN(weight)) AS weight_difference
FROM patients
WHERE last_name = 'Maroni';
```

16 Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.

```
SELECT
  DAY(admission_date) AS day_number,
  COUNT(*) AS number_of_admissions
FROM admissions
GROUP BY day_number
ORDER BY number_of_admissions DESC;
```

17 Show all columns for patient_id 542's most recent admission_date.

```
SELECT *
FROM admissions
GROUP BY patient_id
HAVING
  patient_id = 542
  AND max(admission_date)
```

18 Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:

- 1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
- 2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

```
SELECT
	patient_id,
    attending_doctor_id,
    diagnosis
FROM admissions
WHERE
(
	patient_id % 2 != 0 AND
  	attending_doctor_id IN (1, 5, 19)
)
OR
(
	attending_doctor_id LIKE '%2%' AND
  	LEN(patient_id) = 3
)
```

19 Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.

```
SELECT
  d.first_name,
  d.last_name,
  count(a.admission_date) AS total_admissions
FROM admissions a
JOIN doctors d ON a.attending_doctor_id = d.doctor_id
GROUP BY doctor_id;
```

20 For each doctor, display their id, full name, and the first and last admission date they attended.

```
SELECT
  d.doctor_id,
  CONCAT(d.first_name, ' ', d.last_name) AS full_name,
  MIN(a.admission_date) AS first_admission_date,
  MAX(a.admission_date) AS last_admission_date
FROM doctors d
JOIN admissions a ON d.doctor_id = a.attending_doctor_id
GROUP BY d.doctor_id;
```

21 Display the total amount of patients for each province. Order by descending.

```
SELECT
  pn.province_name,
  COUNT(pa.patient_id) AS total_patients
FROM patients pa
JOIN province_names pn ON pa.province_id = pn.province_id
GROUP BY pn.province_name
order by total_patients DESC;
```

22 For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

```
SELECT
  CONCAT(p.first_name, ' ', p.last_name) AS Patient,
  a.diagnosis AS Diagnosis,
  CONCAT(d.first_name, ' ', d.last_name) AS Doctor
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.attending_doctor_id = d.doctor_id;
```

23 display the first name, last name and number of duplicate patients based on their first name and last name.

Ex: A patient with an identical name can be considered a duplicate.

```
SELECT
  first_name,
  last_name,
  COUNT(*) AS number_of_dublicate_patients
FROM patients
GROUP BY first_name, last_name
HAVING number_of_dublicate_patients > 1;
```

24 Display patient's full name, height in the units feet rounded to 1 decimal, weight in the unit pounds rounded to 0 decimals, birth_date, gender non abbreviated. Convert CM to feet by dividing by 30.48. Convert KG to pounds by multiplying by 2.205.

```
SELECT
	CONCAT(first_name, ' ', last_name) AS full_name,
    ROUND(height / 30.48, 1) AS height_in_feet,
    ROUND(weight * 2.205, 0) AS weight_in_pounds,
    birth_date,
    CASE
    	WHEN gender = 'M' THEN 'Male'
        ELSE 'Female'
	END AS gender_type
FROM patients;
```

25 Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)

```
SELECT
  p.patient_id,
  p.first_name,
  p.last_name
from patients p
  LEFT JOIN admissions a ON p.patient_id = a.patient_id
where a.patient_id IS NULL;
```

### <u>Hard:</u>

1 Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending. For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.

```
SELECT
  COUNT(*) as patients_in_group,
  FLOOR(weight / 10) * 10 AS weight_group
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
```

2 Show patient_id, weight, height, isObese from the patients table.

    - Display isObese as a boolean 0 or 1.
    - Obese is defined as weight(kg)/(height(m)2) >= 30.
    - weight is in units kg.
    - height is in units cm.

```
SELECT
patient_id,
  weight,
  height,
    (CASE
      WHEN weight/POWER(height/100.0, 2) >= 30 THEN 1
    ELSE 0
  END) AS isObese
FROM patients;
```

3 Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'

Check patients, admissions, and doctors tables for required information.

4 Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'

Check patients, admissions, and doctors tables for required information.

```
SELECT
  p.patient_id,
  p.first_name,
  p.last_name,
  d.specialty
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
JOIN doctors d ON a.attending_doctor_id = d.doctor_id
WHERE
  a.diagnosis = 'Epilepsy' AND
  d.first_name = 'Lisa'
```

5 All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

The password must be the following, in order:

1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date

```
SELECT
  p.patient_id,
  CONCAT(p.patient_id, LEN(p.last_name), YEAR(p.birth_date)) AS temp_password
FROM patients p
JOIN admissions a ON p.patient_id = a.patient_id
GROUP BY p.patient_id;
```

6 Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.

```
SELECT
	(CASE
      WHEN patient_id % 2 = 0 THEN 'Yes' ELSE 'No'
	END) AS has_insurance,
	SUM(CASE
      WHEN patient_id % 2 = 0 THEN 10 ELSE 50
	END) AS cost_after_insurance
FROM admissions
GROUP BY has_insurance;
```
