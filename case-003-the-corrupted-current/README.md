# SQL Noir — Case #003: The Corrupted Current
## 📌 Scenario
A body was found floating near the docks of Coral Bay Marina in the early hours of August 14, 1986.
Two individuals near the scene provided the first leads:
- One lived on Ocean Drive
- The other had a last name ending in “ez”

Their interviews hinted at a hotel stay that would become the backbone of the investigation.
My objective was to follow these clues, filter hotel check‑ins, cross‑reference surveillance logs, and confirm the killer using confession records.
---
## 🛠 Skills Demonstrated
- Filtering with 'WHERE'
- Wildcard searches using 'LIKE'
- Logical narrowing with 'AND' and 'IN'
- Cross‑referencing hotel, surveillance, and confession data
- Eliminating false positives through iterative deduction
- Building a narrative from raw data

## 🧩 Investigation Queries (as executed)
### 1) Retrieve the crime scene details
select *
from crime_scene
where location = 'Coral Bay Marina'
  and date = 19860814;
### 2) Identifying the two initial suspects
select *
from person
where address like '%Ocean Drive%';
select *
from person
where name like '%ul%'
  and name like '%ez%';
### 3) Interview the initial suspects
select *
from interviews
where person_id in (101, 102);
### 4) Filter hotel check-ins using both clues
select h.*
from hotel_checkins h
join surveillance_records s
  on h.id = s.hotel_checkin_id
where h.check_in_date = 19860813
  and h.hotel_name like '%Sunset%';
### 5) Identify suspicious behaviors
select *
from surveillance_records
where suspicious_activity IS NOT NULL;
### 6) Narrow to the four suspicious individuals
select *
from surveillance_records
where person_id in (6, 7, 8, 157);
### 7) Retrieve their identities
select *
from person
where id in (6, 7, 8, 157);
### 8) Confirm the final suspect
select *
from confessions
where person_id = 8;
---
## 🕵️ Case Conclusion
**Culprit Identified:** Thomas Brown
