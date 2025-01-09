# Employee_Attrition_Dashboard

### Project Overview
- Created a Tableau dashboard with database integration.
- Performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.
- Developed data models to prepare for analysis, that reducing attrition rates from 15% to 5-10% within 12 months.
- Designed KPIs, charts & calculated fields to analyze attrition trends by department, job role, performance, gender, and age group.

### Objectives
1) Data Import: Imported data from Excel into MySQL and created a database.
2) Data cleaning using queries: COUNT, Rename column, Trim, Change data type, Update records using case statement, Create Primary & Foreign key constraints.
3) Data Analysis: Utilize SQL queries to examine employee data from various perspectives and identify key findings.

#### Project Structure

```sql
use hr_data;

-- table(1) info_data
select * from info_data;

-- table(2) emp_survey_data
select * from emp_survey_data;

-- table(3) man_survey_data
select * from man_survey_data;
```
#### Data Transformation Queries
```sql
-- table(1) info_data
-- COUNT()-Count how many rows are there.
SELECT COUNT(attrition) FROM info_data;

-- Change Column Names:
alter table info_data
rename column EducationField to Edu_Field,
rename column EmployeeCount to Emp_Count,
rename column EmployeeID to Emp_ID,
rename column JobLevel to Job_Level,
rename column JobRole to Job_Role,
rename column MaritalStatus to Marital_Status,
rename column MonthlyIncome to Monthly_Income,
rename column Over18 to Over_18;

-- Trim()-Remove extra whitespaces.
select trim(Emp_ID),
trim(Age),
trim(Attrition),
trim(Department),
trim(Education),
trim(Edu_Field),
trim(Emp_Count),
trim(Gender),
trim(Job_Level),
trim(Job_Role),
trim(Marital_Status),
trim(Monthly_Income),
trim(Over_18),
from info_data;

-- table(2) emp_survey_data
select * from emp_survey_data;

-- Change Column Names:
alter table emp_survey_data
rename column EmployeeID to Emp_ID,
rename column EnvironmentSatisfaction to Envir_Satis,
rename column JobSatisfaction to Job_Satis,
rename column WorkLifeBalance to Work_Life_Bal;

-- Trim() columns:
select trim(Emp_ID),
trim(Envir_Satis),
trim(Job_Satis),
trim(Work_Life_Bal)
from emp_survey_data;

-- table 3: man_Survey_Data
select * from man_survey_data;

alter table man_survey_data
rename column EmployeeID to Emp_ID,
rename column JobInvolvement to Job_Invol,
rename column PerformanceRating to Perf_Rating;

select trim(Emp_ID),
trim(Job_Invol),
trim(Perf_Rating)
from man_survey_data;
```
#### Data Cleaning and Preprocessing Techniques/Queries:
```sql
-- Table 1: info_data
-- col(2) Age
select min(age) from info_data;

select max(age) from info_data;

select age from info_data
where age = 1111111111;

-- col(3) Attrition
select attrition from info_data
where attrition not in ('no', 'yes');

select Attrition from info_data
where Attrition = 'NA';

-- (Yes = Inactive, No = Active)
-- Update multiple records in a column.
update info_data
set Attrition = (case when Attrition = 'Yes' then 'Inactive'
                      when Attrition = 'No' then 'Active'
                      end)
where Attrition in ('Yes', 'No');

-- col(5) Department
select department, count(*) from info_data
group by Department;

select Department from info_data
where Department not in ('sales', 'Research & Development', 'Human Resources');

select Department from info_data
where Department = 'NA';

-- col(7) Education
select education from info_data
where Education = 1111111111;

-- (Before changing the column records, we have to check or change datatype for that column, otherwise it shows datatype error)
-- (eg. Text --> int, int --> text)
-- Change Datatype of "Education" column.
alter table info_data
modify Education text;

select count(distinct education) from info_data;

-- Update multiple records in a column:
update info_data
set Education = (case when Education = 1 then 'Below College'
                      when Education = 2 then 'College'
                      when Education = 3 then 'Bachelor'
                      when Education = 4 then 'Master'
                      when Education = 5 then 'Doctor'
                      end)
where Education in (1, 2, 3, 4, 5);

-- col(8) Edu_Field
select edu_field from info_data
group by Edu_Field;

select edu_field from info_data
where Edu_Field = 'NA';

-- col(9) Emp_Count
select emp_count from info_data
where Emp_Count != 1;

-- col(10) Gender
select gender from info_data
where Gender not in ('Male', 'Female');

-- col(12) job_role
select distinct(Job_Role) from info_data;

-- col(13) Marital_Status
select distinct(marital_status) from info_data;

-- col(14) Monthly_Income
select monthly_income from info_data
where Monthly_Income = 1111111111;

-- col(16) Over_18
select over_18 from info_data
where Over_18 != 'y';

select over_18 from info_data
where Over_18 = 'NA';
```
```sql
-- Table 2: man_survey_data
select * from man_survey_data;

-- col(2) Job_Invol
select count(distinct Job_Invol) from man_survey_data;

-- Change Datatype of "Job_Invol" column
alter table man_survey_data
modify Job_Invol text;

-- Update multiple records in a column
update man_survey_data
set Job_Invol = (case when Job_Invol = 1 then 'Low'
                      when Job_Invol = 2 then 'Medium'
                      when Job_Invol = 3 then 'High'
                      when Job_Invol = 4 then 'Very High'
                      end)
where Job_Invol in (1, 2, 3, 4);

-- col(3) Perf_Rating
select count(distinct Perf_Rating) from man_survey_data;

-- Change Datatype of "Perf_Rating" column:
alter table man_survey_data
modify Perf_Rating text;

-- Update multiple records in a column
update man_survey_data
set Perf_Rating = (case when Perf_Rating = 3 then 'Good'
                        when Perf_Rating = 4 then 'Very Good'
                        end)
where Perf_Rating in (3, 4);
```
```sql
-- Table 3: emp_survey_data
select * from emp_survey_data;

-- col(2) Envir_Satis
select count(distinct Envir_Satis) from emp_survey_data;

-- Change Datatype of "Envir_Satis" column.
alter table emp_survey_data
modify Envir_Satis text;

-- Update multiple records in a column.
update emp_survey_data
set Envir_Satis = (case when Envir_Satis = 1 then 'Low'
                        when Envir_Satis = 2 then 'Medium'
                        when Envir_Satis = 3 then 'High'
                        when Envir_Satis = 4 then 'Very High'
                        when Envir_Satis = 1111111111 then 'NA'
                        end)
where Envir_Satis in (1, 2, 3, 4, 1111111111);

-- col(3) Job_Satis
select * from emp_survey_data;

select distinct Job_Satis from emp_survey_data;

-- Change Datatype of "Work_Life_Bal" column:
alter table emp_survey_data
modify Job_Satis text;

-- Update multiple records in a column.
update emp_survey_data
set Job_Satis = (case when Job_Satis = 1 then 'Low'
                      when Job_Satis = 2 then 'Medium'
                      when Job_Satis = 3 then 'High'
                      when Job_Satis = 4 then 'Very High'
                      when Job_Satis = 1111111111 then 'NA'
                      end)
where Job_Satis in (1, 2, 3, 4, 1111111111);

-- Update specific records with single value.
update emp_survey_data
set Job_Satis = 'Low'
where Emp_ID in (41, 125, 314, 587, 860);

update emp_survey_data
set Job_Satis = 'Medium'
where emp_id in (1196, 1469, 1679, 1910, 2183);

update emp_survey_data
set Job_Satis = 'High'
where emp_id in (2477, 2708, 2876, 3086);

update emp_survey_data
set Job_Satis = 'Very High'
where emp_id in (3296, 3527, 3779, 4031, 4220, 4346);

-- col(4) Work_Life_Bal
select count(distinct Work_Life_Bal) from emp_survey_data;

-- Change Datatype of "Work_Life_Bal" column.
alter table emp_survey_data
modify Work_Life_Bal text;

-- Update multiple records in a column.
update emp_survey_data
set Work_Life_Bal = (case when Work_Life_Bal = 1 then 'Low'
                          when Work_Life_Bal = 2 then 'Medium'
                          when Work_Life_Bal = 3 then 'Good'
                          when Work_Life_Bal = 4 then 'Very Good'
                          when Work_Life_Bal = 1111111111 then 'NA'
                          end)
where Work_Life_Bal in (1, 2, 3, 4, 1111111111);
```
#### Create Primary Key & Foreign Key
```sql
select * from info_data;
select * from emp_survey_data;
select * from man_survey_data;

-- To see the table details.
desc info_data;

-- The below query is used to see the FK Constraint.
show create table info_data;

-- Add/Update Primary Key.
-- table: info_data
alter table info_data
add primary key(emp_id);

-- Add/Update Foreign key.
-- table: man_survey_data
alter table man_survey_data
add foreign key(emp_id) references info_data(emp_id);

-- table: emp_survey_data.
alter table emp_survey_data
add foreign key(emp_id) references info_data(emp_id);
```

