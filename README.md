# WardWatch: Equitable Patient Monitor Allocator

[![Live Demo](https://img.shields.io/badge/demo-live-green.svg)](https://cvee112.github.io/wardwatch/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**WardWatch** is a browser-based scheduling and allocation tool designed for medical wards. It automates the equitable distribution of patient monitoring tasks (vital sign checks) among available staff (doctors/nurses) based on shift duration, patient acuity (frequency of checks), and geographical location within the ward.

The application runs entirely on the client side using vanilla JavaScript and Tailwind CSS, ensuring no patient data is ever sent to an external server.

## 🚀 Key Features

* **Smart Load Balancing:** Utilizes a weighted "Phantom Load" algorithm to distribute patients fairly, accounting for shift length, doctor availability, and current workload.
* **Flexible Shift Configuration:** Support for custom shift start/end times and variable offset start times for Q2, Q4, and Q8 checks.
* **Advanced Workflow Modes:**
    * **Patient Allocation:** Assigns specific patients to specific doctors for the duration of the shift.
    * **Time Blocking:** Switches to a "Block" model where one doctor covers *all* patients in the ward for a specific hour, rotating responsibilities hourly.
* **Workload Smoothing (Staggering):** Automatically offsets check times for patients with the same frequency (e.g., shifting some Q4 checks from 8:00 to 9:00) to prevent massive workload spikes at the top of the hour.
* **Dynamic Staff Availability:** Handle partial shifts, breaks, or overlapping schedules by defining specific availability ranges for each doctor.
* **Acuity-Based Frequency:** Categorize patients by monitoring frequency:
    * **Q1:** Hourly checks
    * **Q2:** Every 2 hours
    * **Q4:** Every 4 hours
    * **Q8:** Every 8 hours
* **Location Clustering:** An adjustable algorithm that prioritizes assigning doctors to patients in the same physical location (e.g., "Left Wing", "Isolation") to minimize travel time.
* **Visual Schedule Generation:**
    * **Card View:** High-level summary of assignments per doctor.
    * **Matrix View:** A spreadsheet-style view of every patient hour-by-hour.
    * **Timeline View:** A chronological breakdown of tasks for the entire shift.
* **State Persistence:** Import and export configuration states via text to save shift setups for future use.

## 🔗 Live Demo

Access the tool directly via GitHub Pages:
**[https://cvee112.github.io/wardwatch/](https://cvee112.github.io/wardwatch/)**

## 🛠️ Usage Guide

### 1. Shift Settings & Strategy
* **Time:** Set the global `Start Time` and `End Time` for the shift.
* **Offsets:** Configure when the first check occurs for Q2, Q4, and Q8 patients (e.g., "Shift Start" vs "Hour 1").
* **Sliders:**
    * *Distribution Logic:* Slide between "Equal Totals" (absolute equality regardless of hours worked) and "Respect Availability" (load scales with hours worked).
    * *Location Clustering:* Increase to penalize splitting a doctor across multiple rooms/bays.
* **Advanced Toggles:**
    * **Time Blocking:** Enable this to assign *hours* to doctors instead of *patients*. This is useful for "Rounding Doctor" models where one person covers the whole floor for an hour.
    * **Stagger Schedule:** Enable this to distribute Q2/Q4/Q8 checks across different hours. For example, instead of checking all Q4 patients at 12 PM, the system will schedule some at 12 PM, some at 1 PM, etc., to smooth out the hourly burden.

### 2. Staff Management
* Enter doctor names in the "Doctors & Preferences" panel.
* **Availability:** Click the clock icon next to a doctor's name to define specific working hours if they are not working the full shift.
* **Preferences:** Assign "Preferred Patients" if continuity of care is required for specific cases.

### 3. Patient Entry
* Paste lists of patient names into the respective acuity columns (Q1, Q2, Q4, Q8).
* Bulk entry is supported (names separated by newlines).

### 4. Locations (Optional but Recommended)
* Define ward locations (e.g., "Bed 1-4", "ICU").
* Assign patients to locations by pasting their names into the location cards. This enables the Clustering algorithm to minimize staff movement.

### 5. Generation & Review
* Click **Generate Schedule**.
* Review the output in the *Results Section*.
* Use the **Export CSV** button in the Table view to save the schedule.

## 🧠 Algorithmic Details

WardWatch uses a customizable heuristic algorithm that adapts based on the selected mode:

### Standard Mode (Patient Allocation)
1.  **Burden Calculation:** Calculates a "Burden Score" for every patient.
2.  **Greedy Allocation:** Iterates through patients (highest burden first) and assigns them to the doctor with the lowest weighted score.
3.  **Score Weighting:** The doctor's score is a composite of:
    * Current Load / Capacity Ratio.
    * **Clustering Bonus:** If the doctor is already in that location, they are favored.
    * **Availability:** Doctors working fewer hours are assigned fewer patients (if "Respect Availability" is high).

### Time Blocking Mode
Instead of assigning patients, the algorithm assigns **Hours**:
1.  **Hourly Burden Analysis:** Calculates the total number of checks required for the entire ward for every hour of the shift.
2.  **Hour Assignment:** Sorts hours by difficulty (heaviest hours first) and assigns them to available doctors, ensuring no doctor is overloaded consecutively if possible.

### Staggering Logic
If enabled, the algorithm applies a modulo offset to patient check times based on their entry order.
* *Example:* For Q4 patients, Patient A is checked at offsets 0, 4, 8... while Patient B is checked at offsets 1, 5, 9... This minimizes "active check" spikes.

## 💻 Running Locally

Since WardWatch is a static web application, it requires no build step or backend server.

1.  Clone the repository:
    ```bash
    git clone [https://github.com/cvee112/wardwatch.git](https://github.com/cvee112/wardwatch.git)
    ```
2.  Navigate to the directory:
    ```bash
    cd wardwatch
    ```
3.  Open `index.html` in any modern web browser.

## 📂 Project Structure

* `index.html`: Contains the entire application logic, UI structure, and embedded CSS/JS.
* *External Dependencies (CDNs):*
    * **Tailwind CSS:** For styling and responsive layout.
    * **FontAwesome:** For UI icons.

## 🤝 Contributing

Contributions are welcome! If you have suggestions for improving the scheduling algorithm or UI enhancements:

1.  Fork the repository.
2.  Create your feature branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
