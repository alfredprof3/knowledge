[[Roster Manager v2 1]]
# 📖 Complete Guide: Student Tracking & Analytics Vault

Welcome to your automated student tracking ecosystem. This system leverages the **Obsidian Templater**plugin to manage a lightweight, relational JSON database behind the scenes, and uses the **Obsidian Dataview** plugin to render live dashboards, contact cards, and academic analytics.

---

## 🛠️ Section 1: System Architecture & Core Logic

Unlike basic markdown files that store text statically, this ecosystem mimics a professional **Relational Database Management System (RDBMS)**. All your data is stored centrally in a single hidden file at the root of your vault: `Attendance-Rosters.json`.

```
                        [ Attendance-Rosters.json ]
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
   "students": {                                  "classes": {
    "ID101":{name:"Alice",                          "Marketing": {
			email:"a@univ.edu"},                       "university":
	"ID102":{name:"Bob",                                 "State College",
			email:"b@univ.edu}                         "semester":
	}                                                    "Spring 2026",
	                                                   "sutdentIds":
	                                                     ["ID101",
	                                                     "ID102"]
	                                                }
	                                              }
```

💎 The Relational Advantage

- **Single Point of Truth:** A student’s metadata (Name, Email, Enrollment ID) is tied strictly to their unique ID.
- **Effortless Rollovers:** When a class graduates or moves to a new course tier (e.g., from _Marketing_ to _Advertising_), the system does not recreate the students. It simply creates a new class registry that references the existing student IDs.
- **Data Integrity:** Dropping a student from an active class removes them from future checklists but preserves their profile globally, preventing historical log archives from breaking.

---

## 📋 Section 2: The Master Tools (Template Scripts)

1️⃣ Roster Manager Template (`Roster Manager`)

The administrative heart of your system. Run this template in any blank note or scratchpad area to manage your global database structure.

🎛️ Menu Explanations & Action Workflows

- **➕ Create a Brand New Class:**
    - _What it does:_ Initializes a new course entry in the database.
    - _Best Practice:_ Use this strictly at the start of a new school term for completely new student intakes.
- **🔄 Rollover/Modify Existing Class (New Semester/Course):**
    - _What it does:_ Clones an existing student pool into a new class title or allows you to reopen an existing class roster.
    - _Use Case 1 (New Term):_ Use this when your current group moves to a higher grade level or takes a different course with you next semester.
    - _Use Case 2 (Missing Entries):_ If you finish creating a class and notice you forgot to input two students, select this option, choose the class, press **Enter** through the metadata prompts to keep them identical, and add the missing students.
- **✏️ Edit Student Info or Class Metadata:**
    - _What it does:_ Fixes clerical errors without using terminal text editors like Vim.
    - _Operational Logic:_ Editing a Student ID automatically sweeps through every class in the database and updates their reference tokens globally so no connections are broken. Renaming a Course Title shifts the entire roster entry seamlessly.
- **❌ Remove Student from a Class:**
    - _What it does:_ Removes a student's ID pointer from a specific course. Use this when a student drops your class mid-term.
- **🗑️ Delete a Class Database:**
    - _What it does:_ Cleans out old academic courses at the end of a 4-to-6 month cycle to keep menus uncluttered.

🛡️ Integrated Safety Gates & "NA" Bypass Logic

Every time you use the **"Register a completely brand new student"** function, the system runs an automated verification scan to block duplicate **Enrollment IDs** and **Student Names**.

For **Email Address Entry**, the system features a smart bypass system:

- **The Problem:** If multiple students lack an email address and you enter placeholders like `"NA"`, the standard duplicate system would halt your entry.
- **The Fix:** The script looks for specialized **Bypass Labels** (`"na"`, `"n/a"`, `"none"`, `"no email"`, `"not applicable"`).
- **The Logic:** If your email input matches any of these keywords (regardless of capitalization), the safety scanner **skips the duplication check entirely**, allowing you to save the profile instantly without annoying alert prompts.

---

2️⃣ Attendance Tracker Template (`Insert Attendance Checklist`)

Run this template inside your daily journal entry or log note to print out a responsive tracking checklist.

- **What it does:** Fetches the class list, counts total registered names, outputs an immutable statistical headcount line (`👥 Total Registered Students: X`), and writes out alphabetical checkboxes.
- **Metadata Footprint:** Every checklist line is stamped with inline parameters:  
    `- [ ] Student Name (student:: Student Name) (class:: Class Name)`

---

3️⃣ Activity Tracker Template (`Insert Activity Checklist`)

Run this template when checking homework, workshops, lab reports, or student projects.

- **What it does:** Prompts you for a specific task title (e.g., `Activity 1. The art of composition`). It pulls your student roster and formats them against that precise title.
- **Metadata Footprint:** Adds a custom assignment token to the line layout:  
    `- [ ] Student Name (student:: Student Name) (class:: Class Name) (activity:: Activity Title)`

---

## 📊 Section 3: Live Analytical Dashboards (DataviewJS)

Place these code blocks inside dedicated notes to turn your static notes into dynamic database queries.

📇 Dashboard A: Searchable Roster Directory

Create a note named `Roster Directory`. This acts as a centralized contact hub.

- **How to use it:** Switch to Obsidian's Reading Mode. It renders clean contact sheets grouped by period.
- **Smart "NA" Formatting:** The code automatically reads student emails. If it detects a bypass label (`"NA"`, `"N/A"`, etc.), it **blocks the generation of a broken hyperlink** and displays the value as clean, italicized plain text (_NA_), making the interface highly polished. Valid emails continue to display as clickable `mailto:` links.
- **Best Practice:** Press `Ctrl + F` / `Cmd + F` on this page to search for any student name, email fragment, or enrollment ID across all classes instantly.

📅 Dashboard B: Weekly Attendance Summary

Create a note named `Weekly Attendance Summary`. It parses all logs created over the trailing 7 days.

- **How it handles marking:**
    - Checked boxes `[x]` are calculated as **✅ Present**.
    - Unchecked boxes `[b]` are calculated as **❌ Absent**.
- **Key Metric Logic:** It features a dynamic denominator tracking exactly when _that specific class_ met. It tallies individual attendance, generates a metric string like `80% (4/5)`, and applies **Conditional Formatting**.
- **Safety Alert:** Any student whose trailing attendance rate drops **below 70%** is wrapped in HTML style blocks that highlight their entire row cell in **bold red with a warning icon (`⚠️`)** for immediate intervention.

📚 Dashboard C: Weekly Activities Summary

Create a note named `Weekly Activities Summary`. It mirrors the analytics engine of your attendance dashboard but indexes columns by your custom task titles rather than calendar dates.

- **Visual Metric:** Tracks student execution workflows, displaying clear **✅ Done** vs **❌ Missing** markers across assignment grids, flag-marking underperforming students in red if their total task submission rate dips below 70%.

---

## 🧠 Section 4: Operational Best Practices

🔄 Forcing Instant Re-loads

Because Dataview tables prioritize hardware performance, they do not read background database file modifications instantly.

- **The Trick:** If you modify a roster or student email, do not waste time clicking back and forth between menus. Simply press **`Ctrl + R` (Windows) or `Cmd + R` (Mac)**. This reloads Obsidian's view engine cache and displays your updated database fields in under two seconds.

📂 Automating Robust Backups

Your entire workflow resides securely inside a single JSON database. To secure your data against hardware failure, your Attendance Tracker template is equipped with a background snapshot system.

- **How it works:** The first time you execute an attendance checklist on any given day, the script scans your vault for a folder named `Roster-Backups`. If it doesn't exist, it creates it, generating a dated archival snapshot (e.g., `Backup-2026-06-02.json`).
- **Emergency Recovery:** If you ever accidentally wipe your database, navigate to your `Roster-Backups`folder, open the newest file, copy the contents, and paste them over your root `Attendance-Rosters.json` file to restore your vault.

---

🚀 Ready to Deploy

To get started, follow these deployment steps:

1. Copy the code blocks we created into their respective templates in your template folder.
2. Bind the `Roster Manager`, `Attendance Tracker`, and `Activity Tracker` templates to customized keyboard hotkeys within **Settings > Hotkeys** (e.g., `Alt + R` for Roster Management).
3. Create your `Roster Directory`, `Weekly Attendance Summary`, and `Weekly Activities Summary` notes to activate your real-time analytics dashboards.

## 🎓 Section 5: End-of-Term Final Report Cards (`Generate Final Report Cards`)

The Report Card engine acts as your automated semester evaluator. Instead of requiring manual calculations at the end of a 4-to-6 month term, this script queries every historical daily note and log entry in your vault to generate comprehensive academic portfolios.

⚙️ How the Engine Computes Data

When you trigger the script and select a course, it runs a background scan using the Dataview API:

1. **Total Sessions (`totalSessions`)**: Calculated by identifying every log file containing checklists marked with your target `class:: Course Name` metadata string that _does not_ possess an `activity::` parameter.
2. **Attendance Tracker (`attendanceData`)**: Tally-counts the total number of checkboxes checked as complete (`[x]`) for each specific student profile.
3. **Activity Tracker (`activityData`)**: Counts unique instances of custom assignment parameters (e.g., `activity:: Activity 1`). It verifies completion states to determine your total active submissions denominator.

---

📥 Step-by-Step Execution Workflow

1. Create a completely blank new note in your archive (e.g., named `Final Grades - Marketing Spring 2026`).
2. Open the **Command Palette** (`Ctrl + P` or `Cmd + P`).
3. Type and run `Templater: Open Insert Template Modal` and select **`Generate Final Report Cards`**.
4. Choose your target class period from the interactive dropdown list.
5. The script will flash an Obsidian confirmation notice and instantly print structural markdown cards for every student on the roster, sorted alphabetically.

---

📊 Understanding the Visual Display Layout & "NA" Logic

Each student receives an independent profile card structured with clean metadata blocks and an analytical grid. If a student profile contains a missing email placeholder, it prints as neat text (`*NA*`) rather than leaving an empty field:

👤 Student: Bob Jones

- **Enrollment ID:** `ID202604`
- **Email:** _NA_

📈 Performance Summary

|Metric|Progress|Percentage|Status|
|---|---|---|---|
|**Class Attendance**|18 / 20 sessions|**90%**|✅ GOOD|
|**Activities Turned In**|5 / 8 tasks|**63%**|❌ INCOMPLETE (Below 70%)|

**Instructor Comments:**

> _[Your cursor lands here automatically so you can append qualitative personalized feedback expressions prior to administrative review]._

🚨 Automatic Status Thresholds

- **✅**: Appears automatically if a student's final rate rests **at or above your strict 70% threshold**.
- **⚠️ CRITICAL / ❌ INCOMPLETE**: Triggers instantly if a student slips **below 70%**, flagging individuals who require remedial support or missed core course milestones.

---

🖨️ Best Practice: Administrative Export

Once the template generates your cards and you type out your custom instructor reviews inside the comment blocks, you can easily share these files outside of Obsidian:

1. Click the **Three Dots Menu (`...`)** in the top-right corner of the active note view.
2. Select **Export to PDF**.
3. Obsidian will export a publication-ready document formatting each student profile cleanly for printing, emailing to your dean, or uploading to your official college system.

## 📊 Section 6: Class Performance Dashboard (`Class Performance Dashboard`)

The Class Performance Dashboard serves as a macro-analytical radar for your entire semester workspace. Instead of drilling into individual student records, it aggregates metrics upward to look at classroom trends.

### ⚙️ Analytical Calculations
1. **Avg Attendance:** Tally-counts every checkbox generated by your Attendance template for that specific course name, dividing overall student presence against total logged seats over the semester.
2. **Avg Submissions:** Aggregates execution data across every task name registered via the Activity checklist, highlighting the collective submission rate of the course.

### 🚨 Administrative Utility
* **Classroom Diagnostics:** Ideal for identifying if a particular course group is showing systematic drop-offs in presence or engagement.
* **Theme-Safe Highlights:** Uses your accessible high-contrast warning logic, automatically color-coding any group average that falls below your target 70% success threshold.

# 📚 System Documentation: Relational Roster Manager

🏗️ Core Concept: The Relational Database

Instead of grouping everything by individual classes, this system splits your data into two separate, connected registries inside your hidden `Attendance-Rosters.json` file:

1. **The Global Student Registry (`students`)**: Stores a unique entry for every student containing their **ID, Name, and Email**. This persists forever.
2. **The Class Registry (`classes`)**: Stores the **Course Name, University, and Semester**. Instead of names, it only stores a list of **IDs**.

_💡 **The Big Advantage:** If you teach a student in three different courses across their college career, their name and email are only typed **once**. The classes simply borrow their ID._

---

🛠️ Main Menu Options

1. ➕ Create a Brand New Class

- **What it does:** Sets up a completely fresh class section.
- **When to use it:** At the start of a brand-new semester when you are teaching a new course title.
- **Workflow:** Prompts you for the Course Title, University, and Semester, then drops you into the **Student Intake Loop** (detailed below).

2. 🔄 Rollover / Modify Existing Class

- **What it does:** Clones an existing roster or adjusts a class layout without deleting past profiles.
- **When to use it:**
    - When a group moves to a new course tier (e.g., _Marketing_ students moving to _Advertising_).
    - **When you accidentally forget to add students** to a class you just made. You select the class, skip renaming prompts by pressing Enter, and jump straight back into the intake loop to add missing names.

3. ✏️ Edit Student Info or Class Metadata

- **What it does:** Corrects mistakes or typos without forcing you to edit the JSON file manually.
- **Sub-options:**
    - **👤 Student Profile:** Edit a name, an email, or an enrollment ID. If you change a student's ID, the script automatically updates that ID across **all** past and present classes simultaneously.
    - **🏫 Class Metadata:** Directly rename a course title, change the university name, or alter the semester string.

4. ❌ Remove Student from a Class

- **What it does:** Drops a student from a specific course list.
- **When to use it:** When a student unenrolls or drops your course mid-term.
- **Note:** This safely stops them from appearing on your daily attendance/activity logs, but **keeps their profile safe** in the global registry so historical records do not break.

5. 🗑️ Delete a Class Database

- **What it does:** Purges a course layout out of the permanent background storage file.
- **When to use it:** At the end of a 4-to-6 month academic period when you no longer need to print active daily logs for that class.

---

🛑 The Student Intake Loop & Safety Gates

When adding students, the script loops through three distinct validation checks to protect you from typos or distractions:

```
[Input Enrollment ID] ➡️ Check 1: Does ID exist? 
	├── Yes ➡️ Prompt: Overwrite profile OR Link to class?
	└── No  ➡️ Proceed to Name Input

[Input Student Name]  ➡️ Check 2: Does Name exist?
	├── Yes ➡️ Alert: Show existing ID. Link profile?
    └── No  ➡️ Proceed to Email Input

[Input Student Email] ➡️ Check 3: Does Email exist?
    ├── Yes ➡️ Alert: Show existing owner. Link profile?
    └── No  ➡️ Successfully Save New Profile!
```

- **ID Protection:** Prevents assigning the same ID token to two different people.
- **Name Protection:** Flags matching names. If it is a different student with a common name, you can choose to bypass it. If it's the same student, you can link them instantly without re-typing their details.
- **Email Protection:** Prevents duplicate communication endpoints.

---

📊 Interaction with Trackers and Dashboards

Because the `Roster Manager` maintains clean entries, your other tools work seamlessly:

- **Checklist Templates:** Read the class ID array, pull the correct names, sort them alphabetically, and print clickable markdown checkboxes with integrated Dataview metadata strings.
- **Dataview Tables:** Scan your daily note checklists and build real-time grids calculating attendance or assignment submission percentages (with automatic warnings if rates drop below 70%).
# 🛠️ Troubleshooting Checklist

When running complex automation scripts in Obsidian, minor bugs can occasionally happen due to plugin updates or accidental formatting issues. Use this quick reference guide to fix common problems:

1. 🟥 Red "Evaluation Error" inside Dataview Tables

- **The Cause:** This happens if your `Attendance-Rosters.json` file is empty, missing, or has corrupted code formatting.
- **The Fix:**
    1. Open the `Roster Manager` template and create a dummy class to force the file to generate correctly.
    2. If the error persists, open your **Command Palette** (`Ctrl/Cmd + P`), type `Dataview: Rebuild Current View`, and press enter to clear the cache.

2. 👥 My Headcount is Correct, but Student Names are Missing

- **The Cause:** You edited a student's ID or deleted a profile, leaving a class referencing an ID that no longer exists in the master registry.
- **The Fix:** Run the `Roster Manager`, select **✏️ Edit Student Info**, and confirm that the student's ID matches the registry. If it is broken, select **🔄 Rollover/Modify Existing Class**, step through the menus, and re-add the student via the **Global Database** selection menu.

3. 🔄 Tables are Not Updating Instantly

- **The Cause:** Obsidian caches background scripts to maximize mobile battery life and performance.
- **The Fix:** You do not need to rewrite any code. Simply press `Ctrl + R` (Windows) or `Cmd + R` (Mac) to reload the application cache. Your tables will refresh immediately.

4. 🔠 Sorting Issues (Alphabetical Order is Wrong)

- **The Cause:** This occurs if a name was accidentally typed with a leading space (e.g., `" Alice"` instead of `"Alice"`).
- **The Fix:** Run the `Roster Manager`, select **✏️ Edit Student Info**, choose **👤 Student Profile**, and check the name string. The script automatically applies a `.trim()` function now, but manually re-saving it will clear old structural whitespace instantly.