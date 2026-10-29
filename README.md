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
11. [Data Visualization](#data-visualization)  
12. [Key Findings](#key-findings)  
13. [Recommendations](#recommendations)
14. [Cross-Track Collaboration Summary](cross-track-collaboration-summary)
15. [Limitations](#limitations)
16. [Assumptions](#assumptions)
17. [Next Steps](#next-steps)
18. [Project Files](#project-files)
19. [Contact Information](#contact-information)

## Project Overview
  This repository documents my work as the Data Analyst on the HealthConnect Clinic Experience Lab, a multidisciplinary project completed as part of the AnalystLab Africa Experience Lab internship programme.
  HealthConnect Clinic is an appointment-based healthcare provider used as a realistic case study for the programme. The project involves helping the clinic use data and AI to improve appointment attendance and patient support.
  As the Data Analyst on this project, my contribution focused on exploring the clinic's appointment data, assessing its quality, identifying patterns behind missed appointments, defining relevant KPIs, and generating actionable business insights. In this project, I also worked with a Data Scientist to build out further modeling and prediction work.
  
## Business Problem
  HealthConnect Clinic loses operational efficiency and patient care capacity because a significant number of scheduled appointments end in no-shows. Staff don't have a clear, data-driven understanding of which patients, appointment types, and booking conditions are most associated with missed visits, and appointment slots freed up by no-shows go unused.
  As a result, the clinic cannot reliably predict attendance risk, target reminders effectively, or plan capacity, leading to wasted clinical resources, longer wait times for other patients, and a weaker patient support experience.

## Central Project Question
How can HealthConnect Clinic use data and AI to understand and reduce missed appointments, and improve how appointment slots and patient support resources are managed?

## Project Questions
* What percentage of appointments end in No-Show / Attended / Cancelled?
* What is the trend of the no-show rate over time?
* How many visits were lost to no-shows in total?
* Which day of the week had the highest no-show rate?
* Which time slot had the highest no-show rate?
* Did appointments booked further in advance have higher no-show rates?
* Which day and time combinations had the worst rates?
* Did no-show rates differ by age group?
* Which gender is at risk of no-show?
* Does distance correlate with no-shows?
* Did patients with more past no-shows also have no-shows this time?
* Did frequent patients no-show less than new patients?
* Which appointment type had the highest no-show rate?
* Does a reminder reduce no-shows?
* Which reminder channel had the lowest no-show rate?
* Was longer waiting time associated with more no-shows?
* How many appointment slots were lost per month?
* Which appointment type lost the most total slots?
* Which combinations of prior no-show history and booking lead time had the highest no-show rates — and does the effect of one factor get stronger or weaker depending on the other?
* Among past appointments, did the relationship between distance to the clinic and no-show rate differ across age groups? Was distance a bigger barrier for some age groups than others?

## Dataset Overview
### Dataset description
The HealthConnect Appointment Dataset is an anonymized dataset representing patient appointment records at HealthConnect Clinic. It captures patient demographics, appointment scheduling details, booking behavior, prior appointment history, reminder activity, distance to the clinic, waiting time, and the final outcome of each appointment (Attended, No-Show or Cancelled).
### Dataset Size
* Records (rows): 5,000 individual appointments
* Variables (columns): 18
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
  The raw HealthConnect appointment data was cleaned and validated using Microsoft Excel Power Query. The original file was not modified. All cleaning was performed in a separate query and exported.
  The raw dataset contained 5,000 records across 18 columns. After cleaning, the record count remained unchanged; no rows were added or removed.
* The raw CSV was loaded into Excel, opening it directly in the Power Query Editor to keep the source file untouched.
* Each column data type were confirmed and two columns (booking_date and appointment_date) stored as text strings were changed to date type.
* `distance_to_clinic_km` (90 missing) and `waiting_time_minutes` (60 missing) were replaced with `"Unknown"` rather than dropped or imputed with a calculated value (e.g. mean/median). This avoids introducing bias into distance- and wait-time-related analysis while preserving the full sample size for other variables.
* Used Power Query's "Remove Duplicates" preview on the full dataset and on `appointment_id` specifically. No duplicate rows or duplicate appointment IDs were found
* Reviewed unique values across `gender`, `age_group`, `appointment_type`, `appointment_day`, `appointment_time`, `reminder_sent`, `reminder_channel`, and `appointment_outcome` for inconsistent spelling, casing, or stray whitespace.
* All categories were already standardized; no corrections were needed
* Saved and closed the clean dataset and then imported it into Power BI for analysis and visualization.

## Importing the Dataset into Power BI
* Opened Power BI Desktop and through the Get Data > Text/CSV, the cleaned dataset was selected
* Since all transformation was don in Excel, the cleaned dataset was loaded directly into Power BI

## Data Analysis
### DAX Measures

```dax
Total Appointments =
COUNTROWS ( HealthConnect_Appointment_Cleaned_Data )

No-Shows =
CALCULATE (
    [Total Appointments],
    HealthConnect_Appointment_Cleaned_Data[appointment_outcome] = "No-Show"
)

No-Show Rate % =
DIVIDE ( [No-Shows], [Total Appointments] )

Attended Rate % =
DIVIDE (
    CALCULATE (
        [Total Appointments],
        HealthConnect_Appointment_Cleaned_Data[appointment_outcome] = "Attended"
    ),
    [Total Appointments]
)

Cancelled Rate % =
DIVIDE (
    CALCULATE (
        [Total Appointments],
        HealthConnect_Appointment_Cleaned_Data[appointment_outcome] = "Cancelled"
    ),
    [Total Appointments]
)

Worst Month Rate =
MAXX (
    VALUES ( 'HealthConnect_Appointment_Cleaned_Data'[appointment_date].[Month] ),
    [No-Show Rate %]
)
```

Total Appointments is the denominator for every rate. No-Show Rate % drives the KPI card and every segment chart. Worst Month Rate is the card on Page 2, and it takes the highest monthly No-Show Rate %.

The outcome donut on Page 1 counts `appointment_id` directly instead of using Total Appointments. I compared the two in Week 7 and they return the same total.

## Data Visualization
### Page 1: Overview Dashboard
Five KPI cards (total appointments, No-Show Rate, Attended Rate %, Cancelled Rate %, No-Shows), the outcome donut (48.46% No-Show, 46.28% Attended, 5.26% Cancelled), the monthly no-show rate, and no-shows by gender. Slicers for appointment type, appointment date and reminder sent filter this page only.
<img width="1453" height="820" alt="Screenshot (265)" src="https://github.com/user-attachments/assets/e0c600ff-3ccf-46ff-9017-4ae324b06b97" />
### Page 2: Timing and Booking Patterns
<img width="1451" height="819" alt="Screenshot (266)" src="https://github.com/user-attachments/assets/285a25b9-a884-4a01-b17d-05dc3b2482dc" />


The Worst Month Rate card (53.85%), a day-by-time matrix, and no-show rate by time, day, age group and lead time. Lead time climbs from 27.81% to 60.49% while day, time and age barely move. The Friday evening cell in the matrix (58.51%) is the highest, but it's a single cell, so don't over-read it.

### Page 3: Patient Risk Profile
<img width="1454" height="813" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/997755f8-533c-4799-aad5-f49d47f7274e" />


Distance by age group, reminder sent against not sent, visit frequency, distance, prior no-shows, and a bubble chart of prior no-shows against rate and volume. Prior no-shows is the cleanest line on the page, rising from 43.51% to 73.33%.

### Page 4: Levers and Capacity Impact
<img width="1467" height="811" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/64a794ce-4210-48e0-933c-a02cf5026dc8" />


No-show rate by appointment type and by reminder channel, no-shows by waiting time, no-shows by month, no-show counts by appointment type, and a prior-no-shows by lead-time matrix. Follow-up has the highest rate (51.23%) but General Consultation loses the most slots (973).

## Key Findings

1. **Booking lead time is the strongest driver of no-shows.** Cramér's V = 0.28, p < 0.0001. The rate climbs close to linearly across five bands, from 29.47% at 0-7 days to 71.36% at 46-60 days.
2. **Prior no-show history is real but weaker alone than it looked.** Point-biserial r = 0.12, p < 0.0001. Its value shows up when you combine it with lead time.
3. **The two factors compound.** 2+ prior no-shows with a 31+ day lead time gives 75.56% no-shows, 3.3 times the lowest-risk group.
4. **Reminders help modestly, and SMS is the best channel.** SMS beats Email and WhatsApp by 2.7 to 4 percentage points. This is my own finding and hasn't been significance-tested.
5. **Age, gender, appointment day and appointment time show no relationship with no-shows.** The p-values are 0.236, 0.401, 0.382 and 0.677.
6. **Rate and count tell different stories.** Follow-up has the highest rate, but General Consultation loses the most slots because it has the most bookings.
7. **Distance is a small effect.** It's statistically real but too small to build a programme around yet.
8. **The model ceiling is a data limit.** Data Science's baseline reaches about 66-67% ROC-AUC, and an independent Random Forest check gave the same ceiling. Better prediction needs new fields, not a fancier algorithm.
9. **The dashboard numbers reconcile.** In Week 7 all five KPI cards, the Worst Month card, the segment charts, three slicers and every filter layer matched manual recalculation. No issues were found, so nothing changed.

## Recommendations

1. **Add a reconfirmation step for bookings made 31+ days ahead**, around the 7-day mark. Risk is concentrated in the 46-60 day range, not spread across all long lead times.
2. **Deploy the two-field high-risk rule now.** Flag patients with 2+ prior no-shows and a 31+ day lead time. A spreadsheet filter or dashboard flag is enough, and it doesn't need a production model.
3. **Move reminders toward SMS** where patient contact preferences allow, and cover the appointments that currently get no reminder at all.
4. **Plan capacity with counts as well as rates.** Use lost-slot volume when deciding where to add overbooking buffers or staff cover.
5. **Don't spend effort targeting by age, gender, weekday or time of day.** None of them showed a real relationship.
6. **Treat distance as secondary** until there's more data on patients who live far away.
7. **Consider shorter booking windows** where clinically appropriate. Lead time is the strongest factor and the one the clinic controls.

## Cross-Track Collaboration Summary

My main partner was **Data Science**. Project Management was the second link, for the final presentation.

**Week 5.** I sent Data Science my final KPI results, the lead-time by prior-no-shows matrix and the reminder-channel findings. I also warned them that distance and waiting time weren't safe to use as model features. That kept the baseline model from relying on those two fields blindly. Evidence: Week 5 report, section 11.

**Week 6.** I sent the same findings as candidate features. Data Science sent back chi-square and Mann-Whitney tests with effect sizes, an independent Random Forest cross-check, the compound risk segment, a five-band lead-time structure and dashboard recommendations. As a result, the lead-time analysis moved from four bands to five, the two-field risk rule was created, four variables were ruled out, and the model's 66-67% ceiling was reframed as a data limit. Evidence: Week 6 Cross-Track Integration Evidence, Data Science's Deep-Dive Report and its notebook.

**Week 7.** I rechecked the segment figures for reminder status, lead time and prior no-shows, and Data Science confirmed which features the candidate model uses. The dashboard figures and the model's feature values matched, so neither side needed to change anything. Evidence: Week 7 Testing & Refinement Report, section 8.

**Week 8.** I handed over the final KPIs, validated findings, insights and recommendations, and Data Science confirmed no model change affects the validated segments. The analytics content went into the final presentation structure with Project Management. Evidence: Week 8 Final Analytics Package, sections 9 and 10.

## Limitations

- **The data is fictional and synthetic.** Effects may be flatter or sharper than in a real clinic, so read the findings as true of this dataset, not as clinical fact.
- **Some segments are small.** 4+ prior no-shows has 15 records and the 31-50 km band has 70. Treat those figures as direction only.
- **The dashboard still uses the Week 5 bands for lead time and distance.** Lead time shows four bands (the 31-60 days bar is 60.49%), not the five-band split Data Science validated. Distance shows only three bands (0-5 km, 25+ km, Unknown) although the data runs up to 45 km, so most appointments don't appear in that chart.
- **The monthly charts combine years.** The data covers Jan 2025 to Jun 2026, so January to June contain two years of appointments and July to December contain one. That's why the no-show count by month on Page 4 drops after June. The rate chart on Page 1 isn't affected by this.
- **Waiting time was misdescribed in earlier reports.** They said it's only recorded for Attended appointments. The dashboard's own waiting-time chart shows 2,386 of the 2,423 No-Shows with a value, and the data dictionary calls it an *estimated* wait. The real issue is that a patient who didn't attend can't have waited, so the field's meaning is unclear and it shouldn't be used as a predictor until that's confirmed.
- **Reminder-channel results haven't been tested statistically.** They come from my own analysis only.
- **Important drivers aren't in the data**: cost of care, transport, weather and the quality of staff-patient communication.
- **All findings are diagnostic.** They describe what already happened, and I did no individual-level prediction.
- **Week 7 testing relied on the cleaned file.** I didn't re-run the earlier cleaning steps, and the segment spot-checks covered representative categories, not every combination.

## Assumptions

- The synthetic dataset is realistic enough to practise real appointment analysis.
- **Cancelled is different from No-Show.** A patient who cancels in advance is treated as operationally different from one who doesn't turn up, so the two are never merged.
- A blank `reminder_channel` means no reminder was sent. It isn't a data error.
- "Prefer not to say" (108 records, 2.2%) stays as its own gender group.
- Missing distances and waiting times are kept as **Unknown**, not imputed, so a data gap isn't dressed up as a real value.
- Patient IDs repeat across appointments by design. The data is appointment-level, not patient-level (1,696 patients, about 2.9 appointments each).
- The cleaned file is correct as provided.

## Next Steps

- **Update the dashboard bands.** Replace the four lead-time bands with the five-band split (0-7, 8-14, 15-30, 31-45, 46-60 days) and rebuild the distance chart so every distance range appears. Then rerun the Week 7 checks.
- **Split the monthly charts by year**, or add a year slicer, so the count by month isn't skewed.
- **Test reminder channel statistically** (SMS against Email against WhatsApp) to confirm or drop the finding.
- **Confirm what `waiting_time_minutes` measures** with clinic operations before using it as a predictor.
- **Add a patient-level table and a live high-risk flag** (2+ prior no-shows, 31+ day lead time) for front-desk use.
- **Work with Data Science on new fields**, such as reminder-response history, cost or insurance, weather and transport access, to get past the 66-67% ROC-AUC ceiling.

## Contact Information
* LinkedIn: https://www.linkedin.com/in/estheraderonke
* Email: aladeloyeesther616@gmail.com
