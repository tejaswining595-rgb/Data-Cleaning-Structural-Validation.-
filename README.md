# Data-Cleaning-Structural-Validation.-
# Data Cleaning & Structural Validation
# Using Python Pandas

import pandas as pd

# ---------------------------------------------------------
# STEP 1: Load the dataset
# ---------------------------------------------------------

# Change the file name if your downloaded dataset has another name
df = pd.read_csv("sample_dataset.csv")

print("========== ORIGINAL DATASET ==========")
print(df)

# ---------------------------------------------------------
# STEP 2: Display basic information
# ---------------------------------------------------------

print("\n========== DATASET INFORMATION ==========")
print(df.info())

print("\n========== NUMBER OF ROWS AND COLUMNS ==========")
print("Rows:", df.shape[0])
print("Columns:", df.shape[1])

# ---------------------------------------------------------
# STEP 3: Check missing values
# ---------------------------------------------------------

print("\n========== MISSING VALUES ==========")
print(df.isnull().sum())

# ---------------------------------------------------------
# STEP 4: Check duplicate records
# ---------------------------------------------------------

print("\n========== DUPLICATE RECORDS ==========")
print("Number of duplicate rows:", df.duplicated().sum())

# Remove duplicate rows
df = df.drop_duplicates()

print("\nDuplicate rows removed.")
print("Number of rows after removing duplicates:", len(df))

# ---------------------------------------------------------
# STEP 5: Standardize column names
# ---------------------------------------------------------

df.columns = (
    df.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)

print("\n========== STANDARDIZED COLUMN NAMES ==========")
print(df.columns.tolist())

# ---------------------------------------------------------
# STEP 6: Remove extra spaces from string columns
# ---------------------------------------------------------

for column in df.select_dtypes(include="object").columns:
    df[column] = df[column].str.strip()

# ---------------------------------------------------------
# STEP 7: Handle missing values
# ---------------------------------------------------------

print("\n========== MISSING VALUES BEFORE CLEANING ==========")
print(df.isnull().sum())

# Fill missing numeric values with median
numeric_columns = df.select_dtypes(include="number").columns

for column in numeric_columns:
    df[column] = df[column].fillna(df[column].median())

# Fill missing text values with "Unknown"
text_columns = df.select_dtypes(include="object").columns

for column in text_columns:
    df[column] = df[column].fillna("Unknown")

print("\n========== MISSING VALUES AFTER CLEANING ==========")
print(df.isnull().sum())

# ---------------------------------------------------------
# STEP 8: Standardize categorical strings
# ---------------------------------------------------------

# Convert text columns to lowercase
for column in text_columns:
    df[column] = df[column].str.lower().str.strip()

# ---------------------------------------------------------
# STEP 9: Convert date columns
# ---------------------------------------------------------

# If the dataset contains a column named "date"
if "date" in df.columns:
    df["date"] = pd.to_datetime(df["date"], errors="coerce")

# If the dataset contains "dob"
if "dob" in df.columns:
    df["dob"] = pd.to_datetime(df["dob"], errors="coerce")

# ---------------------------------------------------------
# STEP 10: Validate data types
# ---------------------------------------------------------

print("\n========== DATA TYPES AFTER CLEANING ==========")
print(df.dtypes)

# ---------------------------------------------------------
# STEP 11: Check missing dates created during conversion
# ---------------------------------------------------------

print("\n========== MISSING VALUES AFTER DATE CONVERSION ==========")
print(df.isnull().sum())

# ---------------------------------------------------------
# STEP 12: Remove completely empty rows
# ---------------------------------------------------------

df = df.dropna(how="all")

# ---------------------------------------------------------
# STEP 13: Reset index
# ---------------------------------------------------------

df = df.reset_index(drop=True)

# ---------------------------------------------------------
# STEP 14: Display cleaned dataset
# ---------------------------------------------------------

print("\n========== CLEANED DATASET ==========")
print(df)

# ---------------------------------------------------------
# STEP 15: Final validation
# ---------------------------------------------------------

print("\n========== FINAL VALIDATION ==========")

print("Number of rows:", df.shape[0])
print("Number of columns:", df.shape[1])

print("\nMissing values:")
print(df.isnull().sum())

print("\nDuplicate rows:")
print(df.duplicated().sum())

print("\nData types:")
print(df.dtypes)

# ---------------------------------------------------------
# STEP 16: Export cleaned dataset
# ---------------------------------------------------------

df.to_csv("cleaned_dataset.csv", index=False)

print("\n==========================================")
print("DATA CLEANING COMPLETED SUCCESSFULLY")
print("Cleaned file saved as: cleaned_dataset.csv")
print("==========================================")