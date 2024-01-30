# Data-Analysis-Skills-Test
# Data Analysis Test

Question 1 - Loading CSV file

# import pandas
import pandas as pd

# read the CSV data from the kaggledata.csv file
df = pd.read_csv('data.csv')



print(df.head())



# Print the list of column names
print("Column Names:", df.columns)



print("Column Names:", df.columns)



Question 2 - Descriptive statistics

Gender

# Extracting gender information from multiple columns
gender_columns = [f'gender{i}' for i in range(1, 9)]  # Adjust the range based on the actual number of gender columns
df['gender'] = df[gender_columns].apply(lambda row: ','.join(row.dropna().astype(int).astype(str)), axis=1)

# Descriptive statistics for 'gender'
gender_column = 'gender'
descriptive_stats_gender = df[gender_column].describe()
gender_counts = df[gender_column].value_counts()
gender_unique_values = df[gender_column].unique()

# Displaying the results
print("Descriptive Statistics for Gender:")
print(descriptive_stats_gender)
print("\nGender Counts:")
print(gender_counts)
print("\nUnique Values for Gender:")
print(gender_unique_values)

Household

# Descriptive statistics for 'hhcode' (household)
household_column = 'hhcode'  # Replace with your actual household column name
descriptive_stats_household = df[household_column].describe()
household_counts = df[household_column].value_counts()
household_unique_values = df[household_column].unique()

# Displaying the results
print("Descriptive Statistics for Household:")
print(descriptive_stats_household)
print("\nHousehold Counts:")
print(household_counts)
print("\nUnique Values for Household:")
print(household_unique_values)

education

# Assuming 'educ' is distributed across 'educ1', 'educ2', ...
educ_columns = [f'educ{i}' for i in range(1, 40) if f'educ{i}' in df.columns]

# Descriptive statistics for education
education_statistics = df[educ_columns].describe()

# Print the descriptive statistics
print("Descriptive Statistics for Education:")
print(education_statistics)

educ_columns = [f'educ{i}' for i in range(1, 39) if f'educ{i}' in df.columns]

# Count of non-null values for each 'educ' column
educ_non_null_counts = df[educ_columns].count()

# Print the non-null counts
print("Non-Null Counts for Education Columns:")
print(educ_non_null_counts)

# Assuming 'married' is the prefix for marital status columns (e.g., married1, married2, ..., married38)
married_columns = [f'married{i}' for i in range(1, 39) if f'married{i}' in df.columns]

# Descriptive statistics for marital status
married_statistics = df[married_columns].describe()

# Counts of each marital status
married_counts = df[married_columns].apply(lambda x: x.value_counts())

# Unique values for marital status
married_unique_values = [df[column].unique() if df[column].notna().any() else [None] for column in married_columns]

# Displaying the results
print("Descriptive Statistics for Marital Status:")
print(married_statistics)

print("\nCounts of Each Marital Status:")
print(married_counts)

print("\nUnique Values for Marital Status:")
for column, unique_values in zip(married_columns, married_unique_values):
    print(f"{column}: {unique_values}")


Question 3
# Step 1: Identify and Handle Missing Values
missing_values = df.isnull().sum()
df = df.dropna()  # You may choose a different strategy based on your analysis

# Step 2: Identify and Handle Inconsistencies
gender_columns = [f'gender{i}' for i in range(1, 40) if f'gender{i}' in df.columns]
unique_values_gender = df[gender_columns].apply(lambda x: x.unique() if not x.empty else pd.NA)

# Step 3: Identify and Handle Outliers
age_columns = [f'age{i}' for i in range(1, 40) if f'age{i}' in df.columns]
outliers_age = df[age_columns].apply(lambda x: (x - x.median()).abs() > 3 * x.std())
df[age_columns] = df[age_columns].where(~outliers_age, other=df[age_columns].median(), axis='columns')  # Specify axis=columns

# Step 4: Additional Transformation
categorical_columns = [f'gender{i}' for i in range(1, 40) if f'gender{i}' in df.columns] + \
                      [f'married{i}' for i in range(1, 40) if f'married{i}' in df.columns]
df[categorical_columns] = df[categorical_columns].astype('category')

# Print cleaned and transformed dataset
print(df.head())

Question 4 (a) Probability calculation

# Assuming 'farmwork' and 'nfarmwork' are prefixes for farm and non-farm work columns
farmwork_columns = [f'farmwork{i}' for i in range(1, 39) if f'farmwork{i}' in df.columns]
nfarmwork_columns = [f'nfarmwork{i}' for i in range(1, 39) if f'nfarmwork{i}' in df.columns]

# Create a new column indicating if at least one member works on both farm and non-farm activities
df['works_on_both'] = (df[farmwork_columns].any(axis=1) & df[nfarmwork_columns].any(axis=1)).astype(int)

# Check if there are any households meeting the criterion
if df['works_on_both'].any():
    # Calculate the probability
    probability_both_activities = df['works_on_both'].mean()
    print("Probability of a household having at least one member engaged in both farm and non-farm activities:", probability_both_activities)
    
    # Assuming a sample size of 100
    sample_size = 100

    # Model the expected number of households
    expected_number_of_households = probability_both_activities * sample_size

    print("Expected number of households meeting the criterion in a sample of 100:", expected_number_of_households)
else:
    print("There are no households in the dataset meeting the criterion.")

# Assuming a sample size of 100
sample_size = 100

# Model the expected number of households
expected_number_of_households = probability_both_activities * sample_size

print("Expected number of households meeting the criterion in a sample of 100:", expected_number_of_households)

4 (b) Mathematical Modelling

# Assuming a sample size of 100
sample_size = 100

# Model the expected number of households
expected_number_of_households = probability_both_activities * sample_size

print("Expected number of households meeting the criterion in a sample of 100:", expected_number_of_households)


Question 5
import matplotlib.pyplot as plt
import seaborn as sns

# Assuming 'married' is the prefix of marital status columns
married_columns = [col for col in df.columns if col.startswith('married')]

# Count the frequency of each marital status category
married_status_counts = df[married_columns].apply(lambda x: x.value_counts()).T.sum()

# Check if there are marital status categories
if not married_status_counts.empty:
    # Create a bar plot
    plt.figure(figsize=(10, 6))
    sns.barplot(x=married_status_counts.index, y=married_status_counts.values, palette='viridis')

    # Set labels and title
    plt.xlabel('Marital Status Categories')
    plt.ylabel('Frequency')
    plt.title('Frequency Distribution of Marital Status Categories')

    # Rotate x-axis labels for better readability
    plt.xticks(rotation=45, ha='right')

    # Show the plot
    plt.show()
else:
    print("No marital status categories found in the dataset.")

