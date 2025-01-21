# Chicago Crime Database Project

## Project Description

This project includes the `Chicago_Crime_Data` database, which stores information about crimes in Chicago, public schools, and socio-economic conditions in various areas of the city. The database contains tables with data on crimes, schools, socio-economic conditions, and a stored procedure for updating school leaders' scores.

## Project Objectives

The main objectives of this project are:
1. **Collecting and analyzing crime data** in Chicago to better understand crime trends, high-crime locations, and types of crimes.
2. **Managing public school data** by updating school leaders' scores (`Leaders_Score`) and assigning appropriate icons (`Leaders_Icon`) based on these scores.
3. **Enabling data analysis** through easy SQL queries, allowing for the identification of crime patterns and the evaluation of preventive measures' effectiveness.

## Tools Used:
- **MySQL (phpMyAdmin)**: For writing queries and managing the database.

## Database Structure

### Tables

1. **chicago_crime**  
   - Table containing information about crimes in Chicago.  
   - Columns: `ID`, `CASE_NUMBER`, `DATE`, `BLOCK`, `IUCR`, `PRIMARY_TYPE`, `DESCRIPTION`, `LOCATION_DESCRIPTION`, `ARREST`, `DOMESTIC`, `BEAT`, `DISTRICT`, `WARD`, `COMMUNITY_AREA_NUMBER`, `FBICODE`, `X_COORDINATE`, `Y_COORDINATE`, `YEAR`, `LATITUDE`, `LONGITUDE`, `LOCATION`.

2. **chicago_public_schools**  
   - Table containing information about public schools in Chicago.  
   - Columns: `School_ID`, `Leaders_Score`, `Leaders_Icon`.

3. **chicago_socioeconomic_data**  
   - Table containing data on socio-economic conditions in various areas of Chicago.  
   - Columns: `Community_Area_Number`, `Community_Area_Name`, `Percent_of_Housing_Crowded`, `Percent_Households_Below_Poverty`, `Percent_Aged_16_Unemployed`, `Percent_Aged_25_Without_Diploma`, `Percent_Aged_Under_18_Over_64`, `Per_Capita_Income`, `Hardship_Index`.

### Stored Procedures

1. **UPDATE_LEADERS_SCORE**  
   - Stored procedure that updates the school leaders' score (`Leaders_Score`) and assigns an appropriate icon (`Leaders_Icon`) based on the score.

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

## Key Insights:

### 1. Crime Trends:
- The most common type of crime is **theft (THEFT)**, especially in high-traffic areas such as shopping centers, streets, and parking lots.
- Crimes related to **domestic violence (DOMESTIC)** account for a significant portion of reports, indicating the need for increased preventive measures in this area.

### 2. High-Crime Locations:
- Areas with high crime rates are mainly **downtown** and surrounding **densely populated neighborhoods**.
- Crimes often occur near **subway stations, bus stops, and public parks**.

### 3. Effectiveness of Police Actions:
- The **arrest rate (ARREST)** is relatively low for crimes such as **theft and property damage**, suggesting the need for increased police patrols in these areas.
- For **drug-related crimes (NARCOTICS)**, the arrest rate is higher, indicating the effectiveness of police actions in this area.
