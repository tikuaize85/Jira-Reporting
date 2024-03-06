# Jira-Reporting

Creating a script to fetch all tasks from a specific sprint in Jira and then plotting their details requires a combination of accessing the Jira REST API and using a data visualization library in Python, such as matplotlib or pandas plotting. Below, I'll outline a generic script that you can adapt to your needs. This script will:

1. Use Jira's REST API to fetch all tasks associated with a specific sprint.
2. Parse the fetched data to extract relevant details.
3. Use matplotlib to create a graphic (e.g., a bar chart) showing these details.


Before you start, ensure you have the necessary Python libraries installed. You might need requests for API calls and matplotlib for plotting. If you don't have them installed, you can install them using pip:

pip install requests matplotlib

Here's a basic script:

import requests
import matplotlib.pyplot as plt
import pandas as pd

# Jira setup - Replace these with your actual credentials and Jira setup
JIRA_URL = 'https://yourdomain.atlassian.net'
JIRA_API_ENDPOINT = '/rest/agile/1.0/sprint/{sprint_id}/issue'
JIRA_SPRINT_ID = 'your_sprint_id_here'
AUTH = ('your_email', 'your_api_token')

# Fetch tasks from a specific sprint
def fetch_issues_from_sprint(sprint_id):
    url = JIRA_URL + JIRA_API_ENDPOINT.format(sprint_id=sprint_id)
    response = requests.get(url, auth=AUTH)
    if response.status_code == 200:
        return response.json()['issues']
    else:
        print("Failed to fetch data:", response.status_code)
        return []

# Extract relevant details from tasks (customize as per your needs)
def extract_task_details(issues):
    tasks_details = []
    for issue in issues:
        task = {
            'Key': issue['key'],
            'Summary': issue['fields']['summary'],
            'Status': issue['fields']['status']['name']
            # Add more fields as needed
        }
        tasks_details.append(task)
    return pd.DataFrame(tasks_details)

# Plot the data
def plot_data(df):
    status_count = df['Status'].value_counts()
    plt.figure(figsize=(10, 6))
    status_count.plot(kind='bar')
    plt.title('Tasks by Status')
    plt.xlabel('Status')
    plt.ylabel('Number of Tasks')
    plt.xticks(rotation=45)
    plt.show()

# Main script
issues = fetch_issues_from_sprint(JIRA_SPRINT_ID)
if issues:
    tasks_details_df = extract_task_details(issues)
    plot_data(tasks_details_df)
else:
    print("No issues found or error in fetching data.")


Please note:

You'll need to replace 'yourdomain', 'your_sprint_id_here', 'your_email', and 'your_api_token' with your actual Jira domain, sprint ID, email, and API token.
The extract_task_details function might require adjustments to extract the exact details you're interested in plotting.
This example creates a simple bar chart showing the number of tasks by their status. You can customize the plot_data function to create different types of charts based on your requirements.
This script is a starting point. Depending on your specific use case, you might want to refine the data extraction and visualization parts.






