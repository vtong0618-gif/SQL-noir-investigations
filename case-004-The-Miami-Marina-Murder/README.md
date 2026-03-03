-- =====================================================
-- SQL Noir Investigation
-- Case: Miami Marina Murder
-- Final Culprit: Mike Manning (ID 105)
-- =====================================================


-- -----------------------------------------------------
-- STEP 1: Identify the crime scene
-- -----------------------------------------------------
SELECT *
FROM crime_scene;


-- -----------------------------------------------------
-- STEP 2: Read witness statements and identify clues
-- (This revealed:
--  - "Meet me at the marina, dock 3."
--  - Invitation ending with "-R"
--  - Navy suit and white tie
-- -----------------------------------------------------
SELECT g.id, g.name, g.occupation, g.invitation_code, w.clue
FROM witness_statements w
JOIN guest g
  ON w.guest_id = g.id;


-- -----------------------------------------------------
-- STEP 3: Narrow suspects using marina clue
-- "Meet me at the marina, dock 3."
-- -----------------------------------------------------
SELECT g.id, g.name, g.invitation_code
FROM marina_rentals m
JOIN guest g
  ON m.renter_guest_id = g.id
WHERE m.dock_number = 3;


-- -----------------------------------------------------
-- STEP 4: Apply invitation clue ("-R")
-- -----------------------------------------------------
SELECT g.id, g.name, g.invitation_code
FROM marina_rentals m
JOIN guest g
  ON m.renter_guest_id = g.id
WHERE m.dock_number = 3
  AND g.invitation_code LIKE '%-R';


-- -----------------------------------------------------
-- STEP 5: Apply attire clue
-- "navy suit" + "white tie"
-- -----------------------------------------------------
SELECT g.id, g.name, a.note
FROM attire_registry a
JOIN guest g
  ON a.guest_id = g.id
WHERE a.note LIKE '%navy%'
  AND a.note LIKE '%white tie%';


-- -----------------------------------------------------
-- STEP 6: Confirm final suspect via confession
-- -----------------------------------------------------
SELECT g.id, g.name, f.confession
FROM final_interviews f
JOIN guest g
  ON f.guest_id = g.id
WHERE g.id IN (105, 34, 91);
