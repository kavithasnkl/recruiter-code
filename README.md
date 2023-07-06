# recruiter-code
import pandas as pd

# Read the data from a CSV file
data = pd.read_excel('Applications_for_Machine_Learning_internship_edited.xlsx - Sheet1.xlsx')


# Drop any duplicate rows
data = data.drop_duplicates()

data.rename(columns={data.columns[6]:'Available'},inplace=True)

# Clean up the data and convert relevant columns to appropriate data types
data['Available'] = data['Available'].str.lower().str.contains('yes')
data['Performance_UG'] = pd.to_numeric(data['Performance_UG'], errors='coerce')
data['Performance_12'] = pd.to_numeric(data['Performance_12'], errors='coerce')
data['Performance_10'] = pd.to_numeric(data['Performance_10'], errors='coerce')

# Define weights for different skills
skill_weights = {
    'Python (out of 3)': 3,
    'Machine Learning (out of 3)': 3,
    'Natural Language Processing (NLP) (out of 3)': 2,
    'Deep Learning (out of 3)': 2
}

# Calculate a score for each candidate based on their skills
data['Skill Score'] = data[['Python (out of 3)', 'Machine Learning (out of 3)',
                            'Natural Language Processing (NLP) (out of 3)', 'Deep Learning (out of 3)']].apply(
    lambda x: sum([x[skill] * skill_weights[skill] for skill in skill_weights.keys()]), axis=1
)

# Select the candidate with the highest skill score and availability
best_candidate = data[data['Available']].sort_values(by='Skill Score', ascending=False).head(1)

# Print the best candidate's information
print('Best Candidate:')
print(best_candidate)
