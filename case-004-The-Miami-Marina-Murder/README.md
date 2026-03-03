# SQL Noir — Case #004: Marina Murder

## 📌 Scenario
At a high-profile marina gathering, a shocking murder took place. 

Witnesses reported suspicious activity near dock 3. One overheard someone say, “Meet me at the marina, dock 3.” Another reported seeing a guest holding an invitation ending in “-R,” dressed in a navy suit with a white tie.

My objective was to analyze witness statements, marina rental records, attire logs, and final interview transcripts to identify and confirm the culprit.

---

## 🛠 Skills Demonstrated
- Multi-table JOIN operations
- Filtering with `WHERE`
- Pattern matching using `LIKE`
- Logical narrowing using layered conditions
- Cross-validating clues across independent tables
- Confirming conclusions using confession records
- Thinking iteratively like a data analyst

---

## 🧩 Investigation Queries (as executed)

### 1) Identify the crime scene
SELECT *
FROM crime_scene;

### 2) Read witness statements and extract structured clues
SELECT g.id, g.name, g.occupation, g.invitation_code, w.clue
FROM witness_statements w
JOIN guest g
  ON w.guest_id = g.id;

### 3) Narrow suspects using marina clue (dock3)
SELECT g.id, g.name, g.invitation_code
FROM marina_rentals m
JOIN guest g
  ON m.renter_guest_id = g.id
WHERE m.dock_number = 3;

### 4) Apply invitation clue ('-R')
SELECT g.id, g.name, g.invitation_code
FROM marina_rentals m
JOIN guest g
  ON m.renter_guest_id = g.id
WHERE m.dock_number = 3
  AND g.invitation_code LIKE '%-R';

### 5) Apply attire clue (navy suit + white tie)
SELECT g.id, g.name, a.note
FROM attire_registry a
JOIN guest g
  ON a.guest_id = g.id
WHERE a.note LIKE '%navy%'
  AND a.note LIKE '%white tie%';

### 6) Confirm final suspect via confession 
SELECT g.id, g.name, f.confession
FROM final_interviews f
JOIN guest g
  ON f.guest_id = g.id
WHERE g.id IN (105, 34, 91);

## 🕵️ Case Conclusion
**Culprit Identified:** Mike Manning (ID 105)

## 🧠 Analyst Takeaways 
This case initially felt ambiguous because each clue independently returned multiple results. “Dock 3” alone produced several guests. The invitation suffix “-R” still left more than one suspect. The attire clue (navy suit, white tie) also matched multiple individuals.
Rather than jumping to conclusions, I approached the problem using incremental filtering and cross-validation:

1. I translated witness statements into structured filters (dock number, invitation suffix, attire description).
2. I applied the marina clue first to reduce the population.
3. I layered the invitation code filter to narrow the pool further.
4. I used the attire registry as an independent verification source.
5. Finally, I confirmed the suspect using the highest-confidence evidence available — the confession record.

The key lesson from this case was that strong analysis is not about writing one complex query. It is about layering evidence, reducing uncertainty step-by-step, and validating conclusions against authoritative data.

This mirrors real-world data work: building confidence through structured narrowing and cross-referenced validation rather than assumption-driven reasoning.
