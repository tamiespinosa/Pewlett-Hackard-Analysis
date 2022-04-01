# Pewlett-Hackard-Analysis


## Table of Contents
- [Overview of Project](#OverviewProject)
  * [Purpose](#purpose)
- [Results](#Results)
  * [Retiring Employees](#retemp)
  * [Employees Eligible for Mentorship Program](#mentp)
- [Summary](#Summary)
- [Resources](#Resources)

## <a name="OverviewProject"></a>Overview of Project

A fictional company, Pewlett Hackard, needed an analysis performed on the employees that would soon be eligible for retirement. The data provided for the analysis included several csv files containing employee, department and management information [[7]](#7). The project required using postgress SQL, and pgAdmin in order to analyze tables, filter them and join tables in order to capture information from different sources. 

### <a name="purpose"></a>Purpose

As part of this project we needed to respond to a few questions:

* Who is retiring in the near future?
* What jobs do the people retiring perform?
* How many people per job title are retiring?
* Who is eligible to participate in a mentorship program? 

## <a name="Results"></a>Results

The data was analyzed using SQL queries [[1]](#1).

### <a name="retemp"></a>Retiring Employees

<a name="version1"></a> **Version 1** 
...

        SELECT e.emp_no, e.first_name, e.last_name,
                ti.title, ti.from_date, ti.to_date
        INTO retirement_titles
        FROM employees AS e
        WHERE e.birth_date BETWEEN '1952-01-01' AND '1955-12-31'
        INNER JOIN titles AS ti
        ON e.emp_no = ti.emp_no
        ORDER BY e.emp_no;

...


[[2]](#2)

...

        SELECT DISTINCT ON (rt.emp_no) 
        rt.emp_no,
                      rt.first_name, 
                      rt.last_name,
                      rt.title

       INTO unique_titles
       FROM retirement_titles AS rt
       WHERE rt.to_date = '9999-01-01'
       ORDER BY rt.emp_no, rt.to_date  DESC;
        
...


[[3]](#3)

...

       SELECT COUNT(ut.emp_no) AS title_count, ut.title
       INTO retiring_titles
       FROM unique_titles as ut
       GROUP BY ut.title
       ORDER BY title_count DESC;

...

[[4]](#4)


### <a name="mentp"></a>Employees Eligible for Mentorship Program

...

       SELECT DISTINCT ON (e.emp_no) 
        e.emp_no, e.first_name, e.last_name, e.birth_date,
               de.from_date, de.to_date,
               ti.title
       INTO mentorship_eligibility
       FROM employees AS e
       INNER JOIN dept_emp AS de
       ON e.emp_no = de.emp_no
       INNER JOIN titles AS ti
       ON e.emp_no = ti.emp_no
       WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
               AND (de.to_date = '9999-01-01')
       ORDER BY e.emp_no;

...


[[5]](#5)



## <a name="Summary"></a> Summary


## <a name="Resources"></a>Resources

<a name="1">[1]</a> [SQL Query](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/blob/d22f8a1007d031e8301fd7e1d28459445a5d6f6f/Queries/Employee_Database_challenge.sql)

<a name="2">[2]</a> [Retirement Titles](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/blob/d22f8a1007d031e8301fd7e1d28459445a5d6f6f/Data/retirement_titles.csv)

<a name="3">[3]</a> [Unique Titles](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/blob/d22f8a1007d031e8301fd7e1d28459445a5d6f6f/Data/unique_titles.csv)

<a name="4">[4]</a> [Retiring Titles](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/blob/d22f8a1007d031e8301fd7e1d28459445a5d6f6f/Data/retiring_titles.csv)

<a name="5">[5]</a> [Membership Program](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/blob/d22f8a1007d031e8301fd7e1d28459445a5d6f6f/Data/mentorship_eligibility.csv)

<a name="6">[6]</a> [Data](https://github.com/tamiespinosa/Pewlett-Hackard-Analysis/tree/main/Data)

[7] https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
