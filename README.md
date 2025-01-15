## Employee_Attrition_Dashboard

![Emp_Attr Dashboard Image](https://github.com/user-attachments/assets/3e50a475-d6ad-45a8-9dd3-a99f861ad8be)


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

-- TABLE(1) info_data
select * from info_data;

-- TABLE(2) emp_survey_data
select * from emp_survey_data;

-- TABLE(3) man_survey_data
select * from man_survey_data;
```
#### Data Transformation Queries
```sql
-- TABLE(1) info_data
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
```
```sql
-- TABLE(2) emp_survey_data
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
```
```sql
-- TABLE(3): man_Survey_Data
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
-- TABLE(1): info_data
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

-- (Before modifying the column records, we must verify or change the data type of that column to avoid a data type mismatch error.)
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
-- TABLE(2): man_survey_data
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
-- TABLE(3): emp_survey_data
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
-- TABLE(1): info_data
alter table info_data
add primary key(emp_id);

-- Add/Update Foreign key.
-- TABLE(2): man_survey_data
alter table man_survey_data
add foreign key(emp_id) references info_data(emp_id);

-- TABLE(3): emp_survey_data
alter table emp_survey_data
add foreign key(emp_id) references info_data(emp_id);
```
#### Questions and Answers

#### Problem Statement
- XYZ company has 4410 employees, with an annual attrition rate of 15%. To improve employee retention, what strategies can be implemented to reduce the attrition rate to 5-10% within the next 12 months, thereby increasing overall employee retention?
- To find the right method to reduce the level of attrition from 15% to 5-10% for the next 12 months.

#### All KPI Count

#### 1. Write a SQL query to count the total number of employees?
```sql
select count(Emp_Count) as Total_Employees from info_data;
```
#### 2. Write a SQL query to calculate the average age of employees who have left the organization?
```sql
select round(avg(Age)) as Total_Attrition_by_Average_Age from info_data
where Emp_ID in
             (select Emp_ID from info_data
              where Attrition = 'Inactive');
```
#### 3. Write a SQL query to calculate the total attrition rate and its percentage value?
```sql
-- (We must use alias (eg. sub) in subquery)
select Total_Employees_Attrition_Count,
       concat(round(100 * (Total_Employees_Attrition_Count / (select count(*) from info_data)), 2), "%") as Percentage
from (select Attrition, 
      count(*) as Total_Employees_Attrition_Count
      from info_data
      where Attrition = 'Inactive') as sub;
```
#### 4. Write a SQL query to calculate the total count of active employees?
```sql
select Total_Active_Employees,
       concat(round(100 * (Total_Active_Employees / (select count(*) from info_data)), 2), "%") as Percentage
from (select Attrition, 
      count(*) as Total_Active_Employees
      from info_data
      where Attrition = 'Active') as sub;
```
#### 5. Write a SQL query to calculate the total attrition count by high-performing employees?
```sql
select count(*) as Total_Attrition_by_High_Performance
from man_survey_data m, info_data i
where i.Emp_ID = m.Emp_ID
and Attrition = 'Inactive'
and Perf_Rating = 'Very Good';
```
#### 6. Write a SQL query to calculate the total attrition count by low-performing employees?
```sql
select count(*) as Total_Attrition_by_Low_Performance
from man_survey_data m, info_data i
where i.Emp_ID = m.Emp_ID
and Attrition = 'Inactive'
and Perf_Rating = 'Good';
```
#### Attrition by different categories
#### 7. Write a SQL query to calculate the total attrition count by age group and gender?
```sql
select case
	   when age between 18 and 25 then '18-25'
	   when age between 26 and 35 then '26-35'
	   when age between 36 and 45 then '36-45'
	   when age between 46 and 55 then '46-55'
	   else '56+'
	   end as Age_Group,
	   gender, count(*) as Employees_Count
	   from info_data
	   where Emp_ID in
                       (select Emp_ID from info_data
                        where Attrition = 'Inactive')
group by Age_Group, Gender
order by Age_Group;
```
#### 8. Write a SQL query to calculate the total attrition count by department?
```sql
select Department, Employees_Count,
       concat(round(100 * (Employees_Count / (select count(*) from info_data)), 2), "%") as Percentage
from (select Department, count(*) as Employees_Count
      from info_data
      where Attrition = 'Inactive'
      group by Department) as sub;
```
#### 9. Write a SQL query to calculate the total attrition count by level of education?
```sql
select Education, count(Education) as Employees_Count from info_data
where Emp_ID in
            (select Emp_ID from info_data
             where Attrition = 'Inactive')
group by Education;
```
#### 10. Write a SQL query to calculate the total attrition count by gender?
```sql
select Gender, Employees_Count,
       concat(round(100 * (Employees_Count / (select count(*) from info_data)), 2), "%") as Percentage
from (select Gender, count(*) as Employees_Count
      from info_data
      where Attrition = 'Inactive'
      group by Gender) as sub;
```
#### 11. Write a SQL query to calculate the total attrition count by salary range?
```sql
select case
	   when Monthly_Income between 10000 and 20000 then '10K-20K'
	   when Monthly_Income between 20001 and 50000 then '20K-50K'
	   when Monthly_Income between 50001 and 100000 then '50K-100K'
           when Monthly_Income between 100001 and 150000 then '100K-150K'
	   else '150K+'
	   end as Salary_Range, count(*) as Employees_Count
	   from info_data
	   where Emp_ID in
                       (select Emp_ID from info_data
	                where Attrition = 'Inactive')
group by Salary_Range
order by Salary_Range;
```
#### 12. Write a SQL query to calculate the total attrition count by job role?
```sql
select Job_Role, count(job_role) as Employees_Count from info_data
where Emp_ID in
            (select Emp_ID from info_data
             where Attrition = 'Inactive')
group by Job_Role;
```
#### 13. Write a SQL query to calculate the total attrition count by field of study?
```sql
select Edu_Field as Education_Field , count(*) as Employees_Count from info_data
where Emp_ID in
            (select Emp_ID from info_data
             where Attrition = 'Inactive')
group by Edu_Field;
```
#### 14. Write a SQL query to calculate the total attrition count by marital status?
```sql
select Marital_Status, Employees_Count,
       concat(round(100 * (Employees_Count / (select count(*) from info_data)), 2), "%") as Percentage
from (select Marital_Status, count(*) as Employees_Count
      from info_data
      where Attrition = 'Inactive'
      group by Marital_Status) as sub;
```
#### 15. Write a SQL query to determine the distribution of job satisfaction ratings across all employees?
```sql
select Job_Satisfaction, Employees_Count, 
       concat(round(100 * (Employees_Count / (select count(*) from info_data)), 2), "%") as Percentage
from (select Job_Satis as Job_Satisfaction, count(*) as Employees_Count
      from emp_survey_data
      group by Job_Satis) as sub
group by Job_Satisfaction
order by Percentage desc;
```
### Findings
- Attrition Trends/Patterns: Examined employee attrition patterns by department, role, performance, age-group, gender, marital status and other relevant dimensions.
- Employee Retention Programs:  Supported 26-35 age-group and low performers with career development workshops, mentorship.
- Focus on Research and Development Field: Offer competitive compensation packages and growth opportunities.
- Reward High Performers: Pay special attention to high-performing employees and offer them challenging assignments, career advancement opportunities, and recognition to aid in retention.

### Conclusion
- This project showcases database connectivity with Tableau, covering data importing, cleaning, EDA, and business-driven SQL queries.
- The findings from this project can help inform business decisions by focusing on targeted retention programs and competitive offers.
- These initiatives supported employee growth and significantly reduced attrition rates.
