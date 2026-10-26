Cleaned data screenshot
 <img width="1501" height="793" alt="cleaned dataset screenshot" src="https://github.com/user-attachments/assets/da62010c-60ab-4c50-89d5-6ab7241472a1" />


Project: Customer Call List Data Cleaning and Preparation
Prepared by: Opute Godswill | Tools: Python, Pandas, Jupyter Notebook | Dataset: Customer Call List 
1. Introduction & Business Context
Data quality determines analytical reliability. This project focuses on cleaning a Customer Call List dataset (20 records) containing CustomerID, First_Name, Last_Name, Phone_Number, Address, Paying Customer, Do_Not_Contact, and Not_Useful_Column.
Initial inspection revealed: duplicate records, inconsistent phone formats (e.g., 123-545-5421 vs 123/643/9775 vs 1235455421), unwanted characters in names (/White, ...Potter), inconsistent categorical labels (Yes/No vs Y/N), combined address fields (street, state, ZIP in one column), and missing values represented as NaN, N/a, NA.
The workflow followed: Raw data → Inspection → Cleaning → Standardization → Missing-value handling → Quality review → Prepared dataset.
While core cleaning operations were demonstrated, final output remains partially cleaned and requires validation before production use.
2. Objectives
The primary goal was to produce a consistent, usable customer list for CRM and analysis. Specific objectives were:
• Identify and remove duplicate records
• Remove irrelevant column (Not_Useful_Column)
• Standardize customer names by stripping unwanted characters
• Clean phone numbers by removing punctuation/symbols
• Split Address into Street_Address, State, Zip_Code
• Standardize Paying Customer and Do_Not_Contact from Yes/No to Y/N
• Handle missing/unavailable values appropriately
• Prepare structured dataset for visualization and reporting
• Document remaining quality issues and improvement recommendations
3. Stakeholders & Tools
Stakeholders:
• CRM Team: Maintains consistent records, identifies incomplete contacts, prepares segments.
• Marketing/Sales: Uses standardized info for campaigns (dataset does not measure campaign performance).
• Customer Service: Easier retrieval of contact details and status.
• Business Management: Assesses database health and need for controls.
• Data Analysts/DBAs: Uses prepared dataset for exploration, migration, reporting.
• Customers: Benefit from fewer errors; Do-Not-Contact preferences must be protected.
Tools Used:
• Python: Automation and reproducibility of cleaning steps.
• Pandas: Core manipulation library – read_excel(), drop_duplicates(), drop(), str.strip(), str.replace() with regex, str.split(), fillna(), replace().
• Jupyter Notebook: Interactive execution and inspection of intermediate outputs.
• Microsoft Excel: Source format for import. No completed Power BI/Tableau dashboard was present in the notebook – dashboard concepts in this report are proposed.
4. Pre-Analysis & In-Analysis Review
Pre-Analysis Board (Before Cleaning):
Dataset overview showed 20 rows with mixed quality. Not_Useful_Column flagged for removal. Missing values visible in Last_Name, Phone_Number, and address components. Data types, missing counts, duplicate counts, and unique CustomerID counts were not systematically documented.
In-Analysis Board (Cleaning Applied):
1. Duplicate Removal: df.drop_duplicates() applied. Removes identical rows but does not catch same customer with different row content. CustomerID uniqueness not fully validated.
2. Column Removal: Not_Useful_Column dropped to simplify structure.
3. Name Standardization: str.strip('/..._') used to clean surnames like /White → White and ...Potter → Potter. Improves readability but only cleans boundaries, not middle characters, and risks over-stripping if not scoped.
4. Phone Cleaning: Regex str.replace('[^a-zA-Z0-9]','') removed dashes/slashes: 123-545-5421 → 1235455421. Retains letters, so placeholders N/a become Na instead of being marked missing. No length/format validation against country numbering plan.
5. Address Separation: str.split(',', n=2, expand=True) split Address into three fields. Works for complete addresses but fails for incomplete entries (e.g., missing state/ZIP). No exception flagging documented.
6. Categorical Standardization: Yes→Y, No→N for Paying Customer and Do_Not_Contact. Requires final validation to ensure all values in {Y,N} and that missing remains distinct.
7. Missing-Value Handling: fillna(' ') and replace of N/a attempted, but replacing with blank space hides missingness and alters types. Final replace of Na was displayed but not assigned back to DataFrame (df['Phone_Number'] = ... missing).
Overall: Notebook demonstrates essential Pandas methods but lacks a final validation report.
5. Data Quality Dimensions
A summary of quality issues observed:
• Uniqueness: Duplicate removal applied; CustomerID repeats need checking.
• Completeness: Missing phone numbers and surnames; incomplete addresses.
• Consistency: Phone numbers used -, /, | separators inconsistently; Yes/No vs Y/N mix.
• Accuracy/Formatting: Names with / and ... symbols; phone numbers with letters from placeholders.
• Structure: Address combined in one field requiring parsing.
• Standardization: Categorical fields inconsistent.
• Relevance: Unnecessary column present.
Example mappings:
Unwanted chars in surname /White → White
Punctuation in surname ...Potter → Potter
Phone format 123-545-5421 → 1235455421
Phone format 123/643/9775 → 1236439775
Yes/No → Y/N
Combined address → Separate columns
Missing NaN → Blank or proper NA handling
6. Dashboard Proposal
Title: Customer Data Quality and Cleaning Dashboard – this is a proposed design, not an existing dashboard.
KPI Cards (to be calculated from validated dataset):
• Total Customer Records
• Duplicate Records Detected
• Missing Phone Numbers
• Invalid Phone Numbers
• Incomplete Addresses
• Contact-Preference Exceptions (Do_Not_Contact invalid/missing)
Visualizations:
• Data Quality Overview – KPI row
• Missing Values by Column – bar chart
• Phone-Number Validation – bar/donut (valid/invalid/missing)
• Customer Status Distribution – Paying Customer Y/N
• Contact Preference Distribution – Do_Not_Contact including missing/invalid
• Cleaning Progress – before/after comparison of duplicates, missing, invalid
Layout: Header → KPI row → Missing-value + Phone validation row → Status distributions row → Exceptions table + Recommendations. Optional filters for Paying Customer and data-quality category.
Tools: After validation, export cleaned CSV/Excel and build visuals in Power BI or Tableau.
7. Observations
• Data inconsistency confirmed across names, phones, and categorical fields.
• Missing information in Last_Name, Phone_Number, and address components.
• Core cleaning methods demonstrated: duplicate removal, column removal, text cleaning, phone standardization, address split, categorical mapping, missing handling.
• Missing-value treatment weak: blank spaces conceal missingness; N/a → Na transformation obscures placeholder.
• Categorical fields lack final quality check for approved values.
• No comprehensive final data-quality report – counts of valid phones, complete records, repeated CustomerIDs not established.
• Scope limited to preparation; no customer segmentation, campaign/sales analysis, or completed dashboard demonstrated.
8. Recommendations for Complete Validation
1. Standardize missing values: Convert '', whitespace, N/a, NA, Na, N/A case-insensitively to pd.NA before cleaning: df.replace(r'^\s*$', pd.NA, regex=True) and explicit placeholder mapping.
2. Validate phone numbers: Check length and numeric format per country rule; do not treat de-punctuated string as valid. Flag missing/incomplete for review; separate placeholder handling from cleaning.
3. Preserve identifiers: Verify CustomerID uniqueness; investigate repeated IDs before deletion to distinguish true duplicates vs same customer with multiple entries.
4. Improve address parsing: Add rules for incomplete addresses; keep original Address, add parsing flags (e.g., Is_Address_Complete, Parse_Status) for manual verification.
5. Standardize categoricals: Trim spaces, upper-case, map Yes/Y → Y and No/N → N, flag other values. Keep missing distinct from Y/N.
6. Protect Do Not Contact: Never default missing Do_Not_Contact to contactable. Require explicit Y/N; escalate unclear to compliance.
7. Create validation report: Document total before/after, duplicates removed, repeated CustomerIDs, missing by column, valid/invalid/missing phones, address completeness, categorical validity.
8. Export validated dataset: After validation, export to Excel/CSV with clear versioning.
9. Build dashboard: Use validated metrics to show actual counts, exceptions, and trends.
10. Establish repeatable workflow: Organize notebook into sections – Import → Inspection (dtypes, missing, duplicates, unique IDs) → Cleaning → Validation → Export → Reporting. Document rules for future loads.
9. Summary & Conclusion
This project examined cleaning and preparation of a Customer Call List using Python, Pandas, and Jupyter Notebook. It addressed common problems: duplicate rows, inconsistent phone formatting, unwanted characters in names, combined address fields, inconsistent Yes/No labels, and missing values.
The notebook provides a foundation and demonstrates relevant Pandas techniques. However, displayed outputs show unresolved items: placeholder handling (N/a → Na), incomplete phone validation, incomplete address splits, and missing-value masking with spaces.
Principal recommendation is to complete validation before considering dataset fully cleaned – including duplicate identifier checks, missing-value audit, phone validity, address completeness, and categorical consistency.
Once validated, the dataset can support CRM maintenance, reporting, and a proposed quality dashboard. In conclusion, the work demonstrates a critical stage of the analytics lifecycle: transforming raw data into more consistent, usable format. With added validation, documentation, and a repeatable workflow, it can serve as a stronger foundation for customer management and data-driven decision-making.
Appendix: Key Techniques
Function / Technique	Purpose
pd.read_excel()	Import Excel dataset
drop_duplicates()	Remove duplicate rows
drop()	Remove unnecessary column
str.strip()	Remove unwanted characters from name boundaries
str.replace() regex	Standardize phone numbers and labels
str.split(',', expand=True)	Separate address into components
fillna() / replace()	Handle missing and placeholder values

