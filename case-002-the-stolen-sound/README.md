# SQL Noir — Case #002: The Stolen Sound

## 📌 Scenario
In 1980s Los Angeles, a prized vinyl record worth over $10,000 was stolen from the West Hollywood Records store.  
The incident occurred on **July 15, 1983**. My objective was to retrieve the crime scene report, follow witness clues, identify the suspect, and confirm via interview transcript.

--- 

## 🛠 Skills Demonstrated
- Filtering with `WHERE`
- Text matching with `LIKE`
- Linking evidence across tables using IDs
- Narrowing suspects using attributes
- Validating conclusions using interview transcripts

---

## 🧩 Investigation Queries (as executed)

### 1) Locate the crime scene report
select *
from crime_scene report
where location like '%Hollywood%'
;
### 2) Retrieve witness records linked to the crime scene
select *
from witnesses
where crime_scene_id = 65
;
### 3) Use witness clues to narrow suspects
select *
from suspects
where bandana_color = 'red'
and accessory = 'gold watch'
;
### 4) Confirm via suspect interviews
select *
from interviews
where suspect_id in (35,44,97)
;
### 5) Identify the culprit
select * 
from suspects
where id = 97
;

-- Culprit: Rico Delgado
