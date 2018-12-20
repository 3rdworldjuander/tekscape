import pandas as pd  
from string import punctuation  

# Read CW Product export  
soitems = pd.read_csv("JH0375.csv")  

# Total ProServ Price  
proserv_price = soitems[soitems['Product Class'].str.contains('Bundle')]['Unit Price'].sum()  

# Total ProServ Cost  
proserv_cost = soitems[soitems['Product Class'].str.contains('Bundle')]['Unit Cost'].sum()  

# Remove leading "-" and spaces from sub-item SKUs  
soitems['Product ID'][:].str.lstrip(punctuation).str.lstrip()  

# Create Sub-items series    
subitems = pd.Series(['PRO-UC-L1', 'PRO-NET-L1', 'PRO-SVR-L1', 'PRO-PMO-L1', 'PRO-SMARTHANDS-L1', 'Risk'])

# Get indexes of rows with subitem SKUs  
soitems['Product ID'].isin(subitems)  

# Output dataframe of all subitem SKUs  
soitems_sub = soitems[soitems['Product ID'].isin(subitems)]  

# Groupby Product ID, Price, and Cost  and get Total Quantity of each grouping  
soitems_sub.groupby(['Product ID', 'Unit Price', 'Unit Cost'])['Quantity'].sum().reset_index(name = 'Total Quantity')  
