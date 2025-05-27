# pr2.8_obd
# ПРАКТИЧНА РОБОТА № 8
# Використання представлень (VIEW)

**Короткий опис:**

У роботі розглянуто основні види обмежень цілісності в реляційних базах даних та способи їх реалізації. Здійснено впровадження декларативного та 
процедурного підходів із врахуванням часу перевірки та області дії обмежень.

**Завдання 2**

VIEW (представлення) — це віртуальна таблиця, яка базується на результаті SQL-запиту. Вона не зберігає дані фізично, але дозволяє спростити складні 
запити, обмежити доступ до певних стовпців або рядків, і створити рівень абстракції.

**Завдання 3: Представлення з іменами тварин**

CREATE VIEW vAnimalNames AS
SELECT AnimalID, Nickname
FROM Animal;

**Завдання 4: Вибір імен тварин з представлення**

SELECT Nickname FROM vAnimalNames;

**Завдання 5: Тварини з ціллю "sale"**

CREATE VIEW vSaleAnimals AS
SELECT *
FROM Animal
WHERE Purpose = 'sale';

**Завдання 6: Оновлення представлення (наприклад, оновити мету)**

UPDATE vSaleAnimals
SET Purpose = 'breeding'
WHERE AnimalID = 1; -- Замініть 1 на наявний AnimalID

**Завдання 7: Представлення з JOIN (тварини + їх доглядачі)**

CREATE VIEW vAnimalCaretakers AS
SELECT A.AnimalID, A.Nickname, C.FullName AS CaretakerName
FROM Animal A
JOIN Caretaker C ON A.AnimalID = C.AssignedAnimalID;

**Завдання 8: Кількість щеплень для кожної тварини**

CREATE VIEW vAnimalVaccinationCounts AS
SELECT A.AnimalID, A.Nickname, COUNT(V.VaccinationID) AS VaccinationCount
FROM Animal A
LEFT JOIN Vaccination V ON A.AnimalID = V.AnimalID
GROUP BY A.AnimalID, A.Nickname;

**Завдання 10: Тварини, що отримали більше 3 щеплень**

CREATE VIEW vFrequentlyVaccinatedAnimals AS
SELECT AnimalID, Nickname, VaccinationCount
FROM vAnimalVaccinationCounts
WHERE VaccinationCount > 3;

**Завдання 11: Додамо Gender до представлення з JOIN**

ALTER VIEW vAnimalCaretakers AS
SELECT A.AnimalID, A.Nickname, A.Gender, C.FullName AS CaretakerName
FROM Animal A
JOIN Caretaker C ON A.AnimalID = C.AssignedAnimalID;

**Завдання 12: Видалення представлення**

DROP VIEW vFrequentlyVaccinatedAnimals;

**Завдання 13: Представлення з псевдонімами (вимірювання ваги)**

CREATE VIEW vWeightSummary AS
SELECT MeasurementID AS ID, Weight AS AnimalWeight, Date AS MeasurementDate
FROM WeightMeasurement;

**Завдання 14: Вага тварини з урахуванням приросту на 10%**

CREATE VIEW vWeightWithProjectedGain AS
SELECT AnimalID, Date, Weight, Weight * 1.10 AS ProjectedWeight
FROM WeightMeasurement;

**Завдання 15: Представлення для важких тварин**

CREATE VIEW vHeavyAnimals AS
SELECT *
FROM WeightMeasurement
WHERE Weight > 150
WITH CHECK OPTION;

Спроба вставки (призведе до помилки)
INSERT INTO vHeavyAnimals (AnimalID, Date, Weight) VALUES (1, '2025-05-01', 100);

**Завдання 16: Зашифроване представлення (може не працювати)**

CREATE VIEW vEncryptedAnimals
WITH ENCRYPTION
AS
SELECT AnimalID, Nickname FROM Animal;

**Завдання 17: Обмежений доступ — представлення для читання ваги**

CREATE VIEW vRestrictedWeights AS
SELECT MeasurementID, Date FROM WeightMeasurement;

Надаємо права користувачу (за умови, що користувач вже існує)
GRANT SELECT ON vRestrictedWeights TO ReadOnlyUser;
