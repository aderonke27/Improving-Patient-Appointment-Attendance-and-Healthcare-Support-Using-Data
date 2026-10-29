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
1. Of 5,000 appointments, 2,423 were no-shows (48.46%), 46.28% were attended and 5.26% were cancelled. More patients skip than show up. That’s far above typical clinic no-show rates, so it’s worth checking the data definitions before presenting it.
2. Only about 5% of patients cancel. Nearly all missed slots are silent no-shows, so the clinic gets no warning and can’t rebook them.
3. The gender split of no-shows is about even (Female 49.73%, Male 48.33%, Prefer not to say 1.94%). Gender isn’t a useful targeting factor. Note these are shares of no-shows, not rates within each gender.
4. The monthly no-show rate swings a lot. July and December peak, while April and November dip hard. The “Worst Month Rate” of 53.85% is probably December.
5. Lead time is the strongest driver. Bookings made 31–60 days ahead no-show at 60.49%, against 27.81% for 0–7 days. The rate falls steadily as the gap shrinks (43.21% for 15–30 days, 33.55% for 8–14 days).
6. Friday evening is the worst slot at 58.51%, followed by Thursday afternoon (53.47%) and Wednesday evening (51.40%). Friday overall looks fine (46.57%), so the evening slot is hiding the problem.
7. Sunday is the worst day overall (50.47%) and Tuesday the best (47.02%). The gap is only about 3 points.
8. Time of day matters little: Morning 48.14%, Afternoon 48.38%, Evening 49.78%.
9. Age shows a small spread. Patients 65+ are the most reliable (45.12%). Those aged 55–64 (50.75%), 25–34 (50.70%) and 18–24 (50.18%) are the least.
10. Past no-shows predict future ones almost in a straight line. The rate goes from 43.51% (none) to 53.49% (1), 59.36% (2), 67.95% (3) and 73.33% (4+). This is the cleanest risk flag in the data.
11. Reminders help but not by much. Patients who got one no-show at 47.36% versus 51.39% for those who didn’t, a gap of about 4 points.
12. Frequent visitors aren’t more reliable. Patients with 6+ visits have the highest rate (53.38%) and those with 0–1 visits the lowest (45.21%). That’s counterintuitive and might point to chronic-care patients who feel less urgency, or to a data quirk.
13. Distance doesn’t work the way you’d expect either. Patients 25+ km away no-show least (46.47%), while those 0–5 km away sit at 49.37%. Patients with unknown distance are worst at 52.22%, which looks like a data capture problem more than a real effect.
14. Some age and distance cells are very high (35–44 with unknown distance at 68.42%, 18–24 at 62.50%). They probably rest on small numbers, so treat them with caution.
15. SMS is the best reminder channel (45.75%), then Email (48.41%) and WhatsApp (49.77%). No reminder is worst at 51.39%. WhatsApp is only 1.6 points better than sending nothing, so it’s underperforming.
16. Follow-ups have the highest no-show rate by type (51.23%) and General Consultation the lowest (46.64%). By volume, though, General Consultation loses the most slots (973), then Follow-up (728), Specialist (427) and Diagnostic (295).
17. Lead time and past no-shows compound each other. Someone with 0 past no-shows booked 0–7 days out misses 21.77% of the time. Someone with 2+ past no-shows booked 31–60 days out misses 71–86% of the time. A few cells hit 100% but are almost certainly tiny samples.
18. Waiting time shows most no-shows (1,297) in the 16–30 minute bucket. This is a count, not a rate, so it mostly reflects where the volume sits. Also, a no-show by definition didn’t wait, so it’s worth checking what this field actually represents.
19. The No-Shows by Month count drops sharply after June. That’s probably the date filter ending at 6/30/2026, so months from July on only have 2025 data. Don’t read it as a real improvement.

## Recommendations
1. Limit long lead times. This is the biggest lever. A 60% no-show rate at 31-60 days versus 28% at 0-7 days means the further out you book, the more slots you lose. Two ways to handle it:
  * Cap routine bookings at around 14 to 30 days where clinically possible.
  * For anything that must be booked far out, require a confirmation at 7 days and again at 48 hours. If the patient doesn’t confirm, release the slot.
2. Make SMS the default reminder. It has the lowest no-show rate (45.75%), and 51.39% of patients with no reminder miss their visit. Send two reminders, one a few days before and one the day before, rather than one. Look into why WhatsApp (49.77%) barely beats sending nothing. It could be message wording, timing, or delivery problems.
3. Fix the data gaps. Before presenting these dashboards to leadership, clean up “Unknown” distance and waiting time. Those fields skew several charts, and the waiting-time chart for no-shows doesn’t make logical sense as it stands.
4. Build a simple risk score. Past no-shows are the cleanest predictor in the data, rising from 43.51% to 73.33%. Combine that with lead time and you can sort every booking into low, medium or high risk. For high-risk patients (2+ past no-shows, booked 30+ days out), use a phone call instead of a text, or ask for a small deposit or confirmation. For the highest tier, overbook those slots so a miss doesn’t leave the clinician idle.
5. Overbook selectively, not across the board. Friday evening (58.51%) and Thursday afternoon (53.47%) are the clear problem slots. Overbooking those two is safer than a clinic-wide policy, since most other slots sit between 44% and 51%.
6. Focus on General Consultation and Follow-ups. General Consultation loses the most slots (973), and Follow-ups have the highest rate (51.23%). For follow-ups, try booking the next visit before the patient leaves, so the gap is short, and tie it to the reminder flow.
7. Leave gender and distance alone. Gender is evenly split and distance runs opposite to what you’d expect, so neither is worth building a program around.
8. Look into frequent visitors. Patients with 6+ visits no-show more (53.38%). Find out whether these are chronic-care patients, and if so, a different reminder or a standing appointment arrangement might suit them better.

## Cross-Track Collaboration: Data Analytics and Data Science
I worked with the Data Science track on one dependency. My dashboard findings (lead time, prior no-shows and reminders) were the candidate features for their no-show model. In return, I needed their statistical testing to know which of my findings were strong enough to act on.

I sent Data Science my final KPI results, the lead-time by prior-no-shows matrix and the reminder-channel findings. I also warned them that distance and waiting time weren't safe to use as model features. Distance showed no clear pattern at the banding I used, and waiting time had unclear meaning. This stopped the baseline model from relying on those two fields blindly.
Evidence: Week 5 report, section 11.

I sent the same findings as candidate features, and Data Science tested them. They ran chi-square and Mann-Whitney tests with effect sizes on every variable, plus an independent Random Forest model as a cross-check.

What the testing showed:
* **Lead time is the strongest driver.** Cramér's V = 0.28, p < 0.0001. Splitting my 31-60 day bar showed 57.03% at 31-45 days and 71.36% at 46-60 days, a jump the single bar hid.
* **Prior no-shows are real but weaker alone.** r = 0.12. The Random Forest agreed that lead time and prior no-shows are the top two features.
* **Combining them gives a sharper signal.** Patients with 2+ prior no-shows and a 31+ day lead time miss 75.56% of appointments (n = 225), against 22.82% for the lowest-risk group (n = 355). That's a 3.3x gap.
* **Four variables showed no relationship with no-shows:** age, gender, appointment day and appointment time (p = 0.236, 0.401, 0.382 and 0.677).
* **The model's 66-67% ROC-AUC ceiling is a data limit.** Both models stalled at the same level, so better prediction needs new fields, not a different algorithm.

What changed for me: my lead-time analysis moved from four bands to five, I added a two-field high-risk rule to my recommendations, and I dropped the four variables from future targeting.
Evidence: Week 6 Cross-Track Integration Evidence, Data Science's Deep-Dive Report and its notebook.

### Week 7: Cross-checking my validated dashboard against the model

I tested my dashboard against the cleaned data, and every KPI and segment check passed. I then gave Data Science my re-validated no-show rates for reminder status, lead time and prior no-shows. They confirmed which features the candidate model uses, and I compared those feature values with my dashboard figures.

Result: the figures matched on the first check, so neither track needed to change anything. This confirmed the dashboard and the model tell the same story before final integration.
Evidence: Week 7 Testing & Refinement Report, section 8.

### Week 8: Final handoff

I handed over my final KPIs, validated findings, insights and recommendations. Data Science confirmed that no model change affects the validated segments. The analytics content went into the final presentation structure with Project Management.
Evidence: Week 8 Final Analytics Package, sections 9 and 10.

### What the collaboration changed

- My findings moved from "this pattern appears in the data" to "this pattern is statistically confirmed, with a measured effect size".
- The clinic gets a high-risk rule it can use now, before any production model exists.
- The analytics dashboard and the predictive model were checked against each other and agree.

### Still open

The Power BI dashboard still shows the four Week 5 lead-time bands. The five-band split comes from Data Science's analysis and is not yet built into the dashboard. This is listed under Next Steps.

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
* Cancelled is different from No-Show. A patient who cancels in advance is treated as operationally different from one who doesn't turn up, so the two are never merged.
* A blank `reminder_channel` means no reminder was sent. It isn't a data error.
* "Prefer not to say" (108 records, 2.2%) stays as its own gender group.
* Missing distances and waiting times are kept as Unknown, not imputed, so a data gap isn't dressed up as a real value.
* Patient IDs repeat across appointments by design. The data is appointment-level, not patient-level (1,696 patients, about 2.9 appointments each).

## Conclusion
This project found that the biggest reason patients miss appointments is how long they wait between booking and the visit. Lead time had the strongest link to no-shows (Cramér's V = 0.28, p < 0.0001), and splitting the 31-60 day band showed the rate jumping from 57.03% at 31-45 days to 71.36% at 46-60 days. Prior no-shows mattered too, but less on their own (r = 0.12). Together they gave the clearest signal: patients with 2 or more prior no-shows and a lead time of 31+ days missed 75.56% of appointments, against 22.82% for the lowest-risk group, a 3.3x gap. Age, gender, appointment day and appointment time showed no relationship with no-shows, so I dropped them from future targeting. The predictive models stopped at a ROC-AUC of 66-67%, and since both models reached the same level, the limit comes from the data. Better prediction will need new fields, not a different algorithm. The dashboard and the model were also checked against each other and told the same story. The clinic can use the two-field high-risk rule right away, without waiting for a production model. Booking patients earlier than necessary is the main thing to avoid, and anyone with a history of no-shows who is booked far ahead should get extra reminders or a follow-up call. The findings come with limits. The data is synthetic, some segments are small, and the dashboard still uses the older lead-time and distance bands. Cost of care, transport, weather and the quality of staff-patient communication aren't in the data. Adding them would be the most useful next step.

## Contact Information
* LinkedIn: https://www.linkedin.com/in/estheraderonke
* Email: aladeloyeesther616@gmail.com
