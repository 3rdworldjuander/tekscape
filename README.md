# Tekscape  
Work Automation Stuff  

# 1. SMARTNet Parser  

## Observations on the new CCW-R quote format  
1. Leading and trailing whitespace on column headers of Quote Details table on 241003158  
2. Macro sheet (2 sheets seen) on 241003158  
3. Missing "Invoice Summary" table title/name on 241003158  
4. 'Extended List Price' column on Quote Details table should be basis of quote but make sure to divide by Quantity before posting to Quosal  


## Extracting multiple tables from CSV  
https://stackoverflow.com/questions/34184841/python-pandas-read-csv-file-containing-multiple-tables?rq=1
### (1) First Approach  
df = pd.read_csv("jahmyst2.csv", header=None, names=range(3))  
table_names = ["Inventory", "HP BladeSystem Rack", "Network Interface"]  
groups = df[0].isin(table_names).cumsum()  
tables = {g.iloc[0,0]: g.iloc[1:] for k,g in df.groupby(groups)}  

>>> list(tables)  
['HP BladeSystem Rack', 'Inventory']  
>>> for k,v in tables.items():  
...     print("table:", k)  
...     print(v)  
...     print()  
...     
table: HP BladeSystem Rack  
              0          1               2  
6   System Name  Rack Name  Enclosure Name  
7      dg-enc05       BU40             NaN  
8  dg-enc05-oa1       BU40        dg-enc05  
9  dg-enc05-oa2       BU40        dg-enc05  

table: Inventory  
                    0             1              2  
1         System Name    IP Address  System Status  
2            dg-enc05           NaN         Normal  
3  dg-enc05_vc_domain           NaN        Unknown  
4        dg-enc05-oa1  172.20.0.213         Normal  

### (2) Second Approach  
df = pd.read_csv('path_to_file')      
index_positions = []  
for table in table_names:  
    index_positions.append(df[df['col_with_table_names']==table].index.tolist()[0])  

## Include end of table for last slice, omit for iteration below  
index_positions.append(df.index.tolist()[-1])  
  
tables = {}  
for position in index_positions[:-1]:  
    table_no = index_position.index(position)  
    tables[table_names[table_no] = df.loc[position:index_positions[table_no+10]]  
