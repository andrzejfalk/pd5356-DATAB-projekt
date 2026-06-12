# pd5356-DATAB-projekt

## Opis projektu

Projekt przedstawia system zarządzania danymi bioinformatycznymi z wykorzystaniem relacyjnej bazy danych MySQL.

W projekcie zaimplementowano relacje:

* Pacjenci → Próbki (1:N)
* Próbki → Badania (1:N)
* Pacjenci ↔ Badania (M:N)
* Badania → Wyniki (1:N)

Dane zostały zaimportowane z plików CSV, a wyniki wybranych analiz wyeksportowano do plików CSV.

## Wyniki zapytań SQL

### Zapytanie 4ii: Testy, w których uczestniczyło więcej niż 30 pacjentów

**Zapytanie**

```sql
SELECT t.test_type, COUNT(DISTINCT pt.patient_id) AS num_patients
FROM tests t
INNER JOIN patient_test pt ON t.test_id = pt.test_id
GROUP BY t.test_type
HAVING COUNT(DISTINCT pt.patient_id) > 30;
```

**Wynik**

| test_type | num_patients |
| --------- | -----------: |
| NGS-panel |           38 |
| SNP Array |           39 |

### Zapytanie 4iii: Średnia liczba wariantów dla każdego testu

**Zapytanie**

```sql
SELECT t.test_type, AVG(variant_counts.num_variants) AS avg_variants
FROM tests t
INNER JOIN (
    SELECT test_id, COUNT(result_id) AS num_variants
    FROM results
    GROUP BY test_id
) AS variant_counts
ON t.test_id = variant_counts.test_id
GROUP BY t.test_type;
```

**Wynik**

| test_type | avg_variants |
| --------- | -----------: |
| SNP Array |       3.0000 |
| NGS-panel |       3.6857 |
| WES       |       3.5294 |
| WGS       |       3.6071 |

## Pliki wynikowe

* patients_tests.csv
* patients_both_tests.csv
* pathogenic_results.csv

