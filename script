import requests
import pandas as pd

tickets_url = 'https://wellspringsupport.zendesk.com/api/v2/tickets'
scout_tickets_url = 'https://wellspringsupport.zendesk.com/api/v2/search.json'

user = [email_here + '/token'
pwd = [token_here]

params = {    
        'query': 'type:ticket group:"Scout"',    
        'sort_by': 'created_at',    
        'sort_order': 'asc'
}

page_count = 0
next_page = ""
raw_scout_data = {}
tickets = []
tickets_list = []
flat_ticket_list = []

#Do all the requests
while next_page != None: 
    response = requests.get(scout_tickets_url, params=params, auth=(user, pwd))
    
    try:
        scout_raw_data = response.json()
    except:
        print("This url isn't working.")
    
    next_page = scout_raw_data.get("next_page")
    page_count = page_count+1

    scout_tickets_url = next_page
    
    #throw out the metadata and dict>list
    tickets = scout_raw_data["results"]

    #append lists to existing lists
    tickets_list.append(tickets)
    
    print("Page ", page_count, " processed.")

#Flatten
for element in tickets_list:
    if type(element) is list:
        # If the element is of type list, iterate through the sublist
        for item in element:
            flat_ticket_list.append(item)
    else:
        tickets_list.append(element)

# Create a DataFrame
df=pd.json_normalize(flat_ticket_list)

# Select only the desired columns
selected_columns = ["url", "id", "via.source.from.address", "created_at", "type", "subject", "description", "status", "recipient"]
df = df[selected_columns]

# Save the DataFrame to a CSV file
df.to_csv('tickets.csv', index=False)
