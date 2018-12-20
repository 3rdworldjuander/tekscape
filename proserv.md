# Import Modules  
import pandas as pd  
from string import punctuation  

# Setting up  

## Create Sub-items series    
subitems = pd.Series(['PRO-UC-L1', 'PRO-NET-L1', 'PRO-SVR-L1', 'PRO-PMO-L1', 'Risk', 'PRO-SMARTHANDS-L1','PRO-SMARTHANDS'])

## Subcon variations series  
subcons = pd.Series(['PRO-SMARTHANDS-L1', 'PRO-SMARTHANDS'])  

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

