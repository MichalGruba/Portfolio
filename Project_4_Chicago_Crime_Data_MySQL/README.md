# Chicago Crime Database Project

## Opis projektu

Ten projekt zawiera bazę danych `Mysql_Learners`, która przechowuje informacje dotyczące przestępstw w Chicago, szkół publicznych oraz warunków społeczno-ekonomicznych w różnych obszarach miasta. Baza danych zawiera tabele z danymi o przestępstwach, szkołach, warunkach społeczno-ekonomicznych oraz procedurę składowaną do aktualizacji wyników liderów szkół.

## Cel projektu

Głównym celem tego projektu jest:
1. **Gromadzenie i analiza danych o przestępstwach** w Chicago, aby lepiej zrozumieć trendy przestępczości, lokalizacje o wysokim wskaźniku przestępczości oraz rodzaje przestępstw.
2. **Zarządzanie danymi szkół publicznych** poprzez aktualizację wyników liderów szkół (`Leaders_Score`) i przypisywanie odpowiednich ikon (`Leaders_Icon`) na podstawie tych wyników.
3. **Umożliwienie analizy danych** poprzez łatwe wykonywanie zapytań SQL, co pozwala na identyfikację wzorców przestępczości oraz ocenę skuteczności działań prewencyjnych.

## Użyte narzędzia:
- **MySQL(phpmyadmin)**

## Struktura bazy danych

### Tabele

1. **chicago_crime**  
   - Tabela zawierająca informacje o przestępstwach w Chicago.  
   - Kolumny: `ID`, `CASE_NUMBER`, `DATE`, `BLOCK`, `IUCR`, `PRIMARY_TYPE`, `DESCRIPTION`, `LOCATION_DESCRIPTION`, `ARREST`, `DOMESTIC`, `BEAT`, `DISTRICT`, `WARD`, `COMMUNITY_AREA_NUMBER`, `FBICODE`, `X_COORDINATE`, `Y_COORDINATE`, `YEAR`, `LATITUDE`, `LONGITUDE`, `LOCATION`.

2. **chicago_public_schools**  
   - Tabela zawierająca informacje o szkołach publicznych w Chicago.  
   - Kolumny: `School_ID`, `Leaders_Score`, `Leaders_Icon`.

3. **chicago_socioeconomic_data**  
   - Tabela zawierająca dane dotyczące warunków społeczno-ekonomicznych w różnych obszarach Chicago.  
   - Kolumny: `Community_Area_Number`, `Community_Area_Name`, `Percent_of_Housing_Crowded`, `Percent_Households_Below_Poverty`, `Percent_Aged_16_Unemployed`, `Percent_Aged_25_Without_Diploma`, `Percent_Aged_Under_18_Over_64`, `Per_Capita_Income`, `Hardship_Index`.
  
### Procedury składowane

1. **UPDATE_LEADERS_SCORE**  
   - Procedura składowana, która aktualizuje wynik liderów szkoły (`Leaders_Score`) oraz przypisuje odpowiednią ikonę (`Leaders_Icon`) na podstawie wyniku.

   ```sql
   CREATE DEFINER=`root`@`%` PROCEDURE `UPDATE_LEADERS_SCORE` (IN `in_School_ID` INT, IN `in_Leader_Score` INT)   
   BEGIN
       START TRANSACTION;

       UPDATE chicago_public_schools
       SET Leaders_Score = in_Leader_Score
       WHERE School_ID = in_School_ID;

       IF in_Leader_Score >= 80 AND in_Leader_Score <= 99 THEN
           UPDATE chicago_public_schools
           SET Leaders_Icon = 'Very strong'
           WHERE School_ID = in_School_ID;
       ELSEIF in_Leader_Score >= 60 AND in_Leader_Score <= 79 THEN
           UPDATE chicago_public_schools
           SET Leaders_Icon = 'Strong'
           WHERE School_ID = in_School_ID;
       ELSEIF in_Leader_Score >= 40 AND in_Leader_Score <= 59 THEN
           UPDATE chicago_public_schools
           SET Leaders_Icon = 'Average'
           WHERE School_ID = in_School_ID;
       ELSEIF in_Leader_Score >= 20 AND in_Leader_Score <= 39 THEN
           UPDATE chicago_public_schools
           SET Leaders_Icon = 'Weak'
           WHERE School_ID = in_School_ID;
       ELSEIF in_Leader_Score >= 0 AND in_Leader_Score <= 19 THEN
           UPDATE chicago_public_schools
           SET Leaders_Icon = 'Very weak'
           WHERE School_ID = in_School_ID;
       ELSE
           ROLLBACK;
       END IF;
       COMMIT;
   END

## Kluczowe wnioski:

### 1. Trendy przestępczości:
- Najczęściej występującym typem przestępstwa jest **kradzież (THEFT)**, szczególnie w obszarach o wysokim natężeniu ruchu, takich jak centra handlowe, ulice i parkingi.

- Przestępstwa związane z **przemocą domową (DOMESTIC)** stanowią znaczącą część zgłoszeń, co wskazuje na potrzebę zwiększenia działań prewencyjnych w tym obszarze.

### 2. Lokalizacje o wysokim wskaźniku przestępczości:
- Obszary o wysokim wskaźniku przestępczości to głównie **centrum miasta** oraz okoliczne **dzielnice o dużej gęstości zaludnienia**.

- Przestępstwa często występują w pobliżu **stacji metra, przystanków autobusowych oraz parków publicznych.**

### 3. Skuteczność działań policyjnych:
- **Wskaźnik aresztowań (ARREST)** jest stosunkowo niski w przypadku przestępstw takich jak **kradzież i uszkodzenie mienia**, co sugeruje potrzebę zwiększenia patroli policyjnych w tych obszarach.

- W przypadku przestępstw związanych z **narkotykami (NARCOTICS)** wskaźnik aresztowań jest wyższy, co wskazuje na skuteczność działań policji w tym obszarze.


