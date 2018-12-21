# Import Modules  

import pandas as pd  
from string import punctuation  

# Setting up  

## Create Sub-items series    
subitems = pd.Series(['PRO-UC-L1', 'PRO-NET-L1', 'PRO-SVR-L1', 'PRO-PMO-L1', 'Risk', 'PRO-SMARTHANDS-L1','PRO-SMARTHANDS'])

## Subcon variations series  
subcons = pd.Series(['PRO-SMARTHANDS-L1', 'PRO-SMARTHANDS'])  

## Create output template  
template = {
'Product ID' : 
['PKG-PRO-PROFSVCS', 'PRO-NET-L1', 'PRO-SVR-L1', 'PRO-UC-L1', 'PRO-PMO-L1', 'PRO-SMARTHANDS-L1', 'Risk', 'PKG-PRO-PROFSVCS', 'PKG-PRO-PROFSVCS', 'PKG-PRO-PROFSVCS']
,
'Quantity' : 
[1, 0, 0, 0, 0, 0, 0, 1, 1, 1]
,
'Price' :
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
,
'Cost' :
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
,
'Customer Description' :
['Professional Services: 50%', '', '', '', '', '', '', 'Professional Services: 20%', 'Professional Services: 20%', 'Professional Services: 10%']
}

out_table = pd.DataFrame(template, columns=['Product ID', 'Quantity', 'Price', 'Cost', 'Customer Description'])

# Main app  

## Read CW Product export  
soitems = pd.read_csv("filename.csv")  

## Total ProServ Price  
proserv_price = soitems[soitems['Product Class'].str.contains('Bundle')]['Unit Price'].sum()  

## Total ProServ Cost  
proserv_cost = soitems[soitems['Product Class'].str.contains('Bundle')]['Unit Cost'].sum()  

## Remove leading "-" and spaces from sub-item SKUs  
soitems['Product ID'] = soitems['Product ID'][:].str.lstrip(punctuation).str.lstrip()  

## Output dataframe of all subitem SKUs  
soitems_sub = soitems[soitems['Product ID'].isin(subitems)]  

## Groupby Product ID, Price, and Cost  and get Total Quantity of each grouping  
sub_agg = soitems_sub.groupby(['Product ID', 'Unit Price', 'Unit Cost'])['Quantity'].sum().reset_index(name = 'Total Quantity')  

