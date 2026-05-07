# Project 5 - Tension Member Yielding Check Tool

## Summary

This project evaluates steel tension members for gross-section yielding using both:

- LRFD design checks
- ASD design checks

The code reads steel shape properties from `W_Shape_Table.csv` and material yield stresses from `ASTM_Material_Fy_Table.csv`, then calculates factored demand, allowable strength, and percent utilization for several design scenarios.

The workflow includes:

- one user-defined case entered through prompts
- three predefined comparison scenarios
- tabulated results for all scenarios
- bar charts showing LRFD and ASD utilization

This project is useful for comparing how different W-shapes, materials, and loading conditions perform under tensile yielding requirements.

---

## Repository Contents

- `Project 5 Group GEST-01-C Codebase.ipynb` - main notebook containing the full analysis and code
- `W_Shape_Table.csv` - steel shape table used to retrieve gross area, `Ag`
- `ASTM_Material_Fy_Table.csv` - ASTM material table used to retrieve yield stress, `Fy`
- `Project 5 Group GEST-01-C - Flow Chart.pdf` - workflow diagram for the project process
- `Project 5 Group GEST-01-C - SOW 2.docx` - scope of work and project planning document
- `Project 5 Group GEST-01-C Gantt Chart.xlsx` - project schedule and task timeline
- `Timesheet.xlsx` - project time tracking record
- project report - full written report describing methods, results, and discussion
- technical executive summary - concise technical summary for an engineering audience
- non-technical executive summary - concise summary for a general audience

Only the notebook and the two CSV files are required to run the code. The remaining documents support project documentation and deliverables.

---

## Engineering Purpose

The code checks tensile yielding using the standard gross-section relationship:

- Nominal tensile strength: `Pn = Fy * Ag`
- LRFD available strength: `φPn = 0.90 * Pn`
- ASD allowable strength: `Pallow = Pn / 1.67`

It also computes the required load effects:

- LRFD demand: `Pu = 1.2D + 1.6L`
- ASD demand: `Pa = D + L`

The program reports whether each scenario passes or fails under both design methods.

---

## Requirements

- Python 3.8+
- Required libraries:
  - `pandas`
  - `numpy`
  - `matplotlib`
  - `seaborn`

Install dependencies with:

```bash
python -m pip install pandas numpy matplotlib seaborn
```

---

## Files Needed to Run

The following files must remain in the same working directory when the code is executed:

- `Project 5 Group GEST-01-C Codebase.ipynb`
- `W_Shape_Table.csv`
- `ASTM_Material_Fy_Table.csv`

If the CSV files are missing or moved, the program will not be able to find shape areas or material strengths.

---

## How to Run

### Option 1 - Run the Notebook

1. Open `Project 5 Group GEST-01-C Codebase.ipynb`.
2. Make sure both CSV files are in the same folder as the notebook.
3. Start the notebook kernel.
4. Run the cells from top to bottom.
5. When prompted, enter the required user input values.

---

## Required User Inputs

The program asks for one custom design case.

### 1. W-Shape Designation

Example:

```text
W12X50
```

This must match a valid shape listed in `W_Shape_Table.csv`.

### 2. ASTM Material

Examples:

```text
A36
A572 Gr.50
A992
```

If the user leaves this blank, the code automatically uses:

```text
A992
```

### 3. Dead Load, `D` (kips)

Example:

```text
120
```

### 4. Live Load, `L` (kips)

Example:

```text
80
```

The code validates the shape and material names and requires the load inputs to be nonnegative numeric values.

---

## Built-In Comparison Scenarios

After the user-defined case is entered, the program automatically evaluates three additional scenarios:

```python
Scenario 1: W14X90, A572 Gr.50, D = 150.0 kips, L = 95.0 kips
Scenario 2: W10X30, A36,       D = 90.0 kips,  L = 60.0 kips
Scenario 3: W16X57, A992,      D = 110.0 kips, L = 75.0 kips
```

These scenarios allow the user case to be compared against preset design examples.

---

## Code Workflow

The program runs in this order:

1. Import Python libraries.
2. Load steel shape data from `W_Shape_Table.csv`.
3. Load material yield stress data from `ASTM_Material_Fy_Table.csv`.
4. Prompt the user for a custom design case.
5. Retrieve `Ag` and `Fy` values from the CSV files.
6. Compute LRFD and ASD tensile yielding checks.
7. Evaluate the three built-in scenarios.
8. Combine all results into a table.
9. Plot LRFD utilization by scenario.
10. Plot ASD utilization by scenario.

---

## Outputs Generated

The code produces three main outputs.

### 1. Printed Results Table

The terminal or notebook displays a formatted table with the following fields:

- `Scenario`
- `Shape`
- `Material`
- `Ag (in^2)`
- `Fy (ksi)`
- `Pu (kips)`
- `Pa (kips)`
- `φPn (kips)`
- `Pn/Ω (kips)`
- `LRFD Utilization (%)`
- `ASD Utilization (%)`
- `LRFD Pass`
- `ASD Pass`

### 2. LRFD Utilization Plot

A bar chart showing the LRFD utilization percentage for each scenario.

### 3. ASD Utilization Plot

A bar chart showing the ASD utilization percentage for each scenario.

Both plots include a red dashed line at `100%`, which represents the design limit.

---

## How to Interpret the Results

### Pass / Fail Logic

- `PASS` means the available strength is greater than or equal to the required demand.
- `FAIL` means the required demand exceeds the available strength.

### Utilization Percentage

Utilization indicates how much of the member capacity is being used.

- Less than `100%` = acceptable
- Equal to `100%` = exactly at the limit
- Greater than `100%` = overstressed

Examples:

- `65%` utilization means the member is comfortably within capacity.
- `98%` utilization means the member is close to the design limit.
- `120%` utilization means the member fails the check.

### LRFD vs ASD

- LRFD compares factored loads against reduced nominal strength.
- ASD compares service loads against allowable strength.

Looking at both methods helps the user understand whether a section is acceptable under each design philosophy.

---

## Common Input Issues

### Invalid Shape

If a shape is not found, the code will ask the user to try again. The designation must match a valid W-shape in the CSV file.

### Invalid Material

If the material is not found in the ASTM table, the code will ask for another material name.

### Negative or Non-Numeric Loads

If the user enters a negative value or text instead of a number, the code will continue prompting until a valid number is entered.

---

## Example Use Case

A user may want to check whether a selected W-shape made from a chosen ASTM material can safely resist a given dead load and live load.

For example, the user might enter:

```text
Shape: W12X50
Material: A992
Dead Load: 120
Live Load: 80
```

The program then:

- looks up the gross area of `W12X50`
- looks up the yield stress for `A992`
- computes LRFD and ASD tensile yielding capacities
- compares demand to capacity
- reports utilization percentages and pass/fail status
- compares that result with three preset scenarios

---

## Notes

- This project checks gross-section tensile yielding only.
- It does not include fracture checks, block shear checks, or connection design.
- The code assumes the CSV tables contain the correct shape and material properties.
- The default material is `A992` when the material field is left blank.

---

## Recommended Use

This code is best used as:

- a teaching example for LRFD and ASD tension member checks
- a quick comparison tool for different steel shapes and ASTM materials
- a demonstration of how tabular steel data can be connected to design calculations in Python

---

## Main Takeaway

This project turns steel section data and material properties into a repeatable tensile yielding check that is easy to run, compare, and visualize. A user can enter one custom case, review three additional scenarios, and quickly determine whether each member passes or fails under LRFD and ASD criteria.
