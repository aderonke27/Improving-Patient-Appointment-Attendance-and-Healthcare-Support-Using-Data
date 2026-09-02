# Improving-Patient-Appointment-Attendance-and-Healthcare-Support-Using-Data-And-AI
![Power BI](https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat&logo=powerbi&logo)
![Domain](https://img.shields.io/badge/Domain-Healthcare%20Analytics-purple)

## Table of Contents
1. [Project Overview](#project-overview)  
2. [Business Problem](#business-problem)
3. [Central Project Question](#central-project-question)
4. [Project Questions](#project-questions)
5. [Dataset Overview](#dataset-overview)
6. [Data Quality Assessment](#data-quality-assessment)
7. [Tools Used](#tools-used)
8. [Data Cleaning and Transformation in Excel](#data-cleaning-and-transformation-in-excel) 
9. [Importing the Dataset into Power BI](#importing-the-dataset-into-power-bi)  
10. [Data Analysis](#data-analysis)
11. [KPIs Tracked](#kpis-tracked)
12. [Data Visualization](#data-visualization)  
13. [Key Findings](#key-findings)  
14. [Recommendations](#recommendations)  
15. [Limitations](#limitations)
16. [Assumptions](#assumptions)
17. [Next Steps](#next-steps)
18. [Contact Information](#contact-information)

## Project Overview
This repository documents my work as the Data Analyst on the HealthConnect Clinic Experience Lab, a multidisciplinary project completed as part of the AnalystLab Africa Experience Lab internship programme.

HealthConnect Clinic is a fictional appointment-based healthcare provider used as a realistic case study for the programme. The project simulates a real-world data team, with interns working across five professional tracks — Project Management, Data Analytics, Data Science, Machine Learning Engineering, and Generative AI — all contributing to one shared objective: helping the clinic use data and AI to improve appointment attendance and patient support.

As the Data Analyst on this project, my contribution focuses on exploring the clinic's appointment data, assessing its quality, identifying patterns behind missed appointments, defining relevant KPIs, and generating actionable business insights. This work forms the analytical foundation that other tracks — particularly Data Science — build on for further modeling and prediction work.

## Business Problem
HealthConnect Clinic loses operational efficiency and patient care capacity because a significant number of scheduled appointments end in no-shows. Staff don't have a clear, data-driven understanding of which patients, appointment types, and booking conditions are most associated with missed visits, and appointment slots freed up by no-shows go unused.

As a result, the clinic cannot reliably predict attendance risk, target reminders effectively, or plan capacity — leading to wasted clinical resources, longer wait times for other patients, and a weaker patient support experience.

## Central Project Question
How can HealthConnect Clinic use data to understand and reduce missed appointments, and improve how appointment slots and patient support resources are managed?

## Project Questions

## Dataset Overview
### Dataset description
The HealthConnect Appointment Dataset is a fictional, anonymized dataset representing patient appointment records at HealthConnect Clinic. It captures patient demographics, appointment scheduling details, booking behavior, prior appointment history, reminder activity, distance to the clinic, waiting time, and the final outcome of each appointment (Attended, No-Show or Cancelled).
### Dataset Size
* Records (rows): 5,000 individual appointments
* Variables (columns): 18
* Unique patients: 1,696 (an average of roughly 2.9 appointments per patient, confirming the dataset is appointment-level, not patient-level, and that most patients appear more than once)
* Appointment ID range: HC-00001 to HC-05000 (no gaps or duplicate IDs found)
* Date coverage: booking dates from November 2024 to June 2026; appointment dates from January 2025 to June 2026
### Variable categories
The 18 variables fall into six broad categories relevant to the no-show problem:
*	Patient demographics: gender, age, age_group
*	Appointment details: appointment_id, appointment_type, appointment_dav, appointment_time
*	Booking information: booking_date, appointment_date, booking_lead_days
*	Patient history: previous_appointments, previous_no_shows
*	Reminder information: reminder_sent, reminder_channel
*	Clinic logistics: distance_to_clinics_km, waiting_time_minutes
*	Outcome: appointment_outcome 
### Data source
The dataset was provided by AnalystLab Africa as an official project resource for the HealthConnect Experience Lab. It is explicitly fictional and synthetic, generated to simulate realistic clinic appointment data for learning purposes. No real patient information is contained in the file.

## Data Quality Assessment
* In the raw CSV file, booking_date and appointment_date were stored as text strings instead of date type. All other data types were correct.
* Two variables contained missing values; distance_to_clinic_km and waiting_time_minutes. 
* No duplicates were found in the dataset.
* The dataset exhibited high format uniformity and consistency.

## Tools Used
* Microsoft Excel
* Power query
* Microsoft Power BI

## Data Cleaning and Transformation in Excel

## Importing the Dataset into Power BI

## Data Analysis
### DAX Measures
### KPIs Tracked

## Data Visualization

## Key Findings

## Recommendations

## Limitations

## Assumptions

## Next Steps

## Contact Information
* LinkedIn: https://www.linkedin.com/in/estheraderonke
* Email: aladeloyeesther616@gmail.com
