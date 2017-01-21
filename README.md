# JSON-Chapter-3.3-Exercises-for-Data-Science-Intensive-Class


# Problem 1:  Find the 10 countries with most projects
sample_json_df = pd.read_json('data/world_bank_projects.json')
sample_json_df.groupby('countryshortname').size().sort_values(ascending=False).head(10)

countryshortname
Indonesia             19
China                 19
Vietnam               17
India                 16
Yemen, Republic of    13
Nepal                 12
Bangladesh            12
Morocco               12
Mozambique            11
Africa                11





# Problem 2:  Find the top 10 major project themes (using column 'mjtheme_namecode')
df = pd.read_json('data/world_bank_projects.json')

all_codes = df.mjtheme_namecode[0]    # extract out just the json column
for x in range(1,len(df)) :           # combine all the json rows together
    all_codes = all_codes + df.mjtheme_namecode[x]  
    
all_codes_normalized = json_normalize(all_codes).sort_values(['code','name']) # normalize all together and sort
all_codes_normalized.name = all_codes_normalized.name.replace('', np.nan, regex=True).fillna(method='bfill') # replace empty strings and backfill correct strings
all_codes_normalized.groupby(['code','name']).size().sort_values(ascending=False).head(100)  #groupby and output final results

code  name                                        
11    Environment and natural resources management    250
10    Rural development                               216
8     Human development                               210
2     Public sector governance                        199
6     Social protection and risk management           168
4     Financial and private sector development        146
7     Social dev/gender/inclusion                     130
5     Trade and integration                            77
9     Urban development                                50
1     Economic management                              38
3     Rule of law                                      15




# Problem 3:  In 2. above you will notice that some entries have only the code and the name is missing. Create a dataframe with the missing names filled in.

df = pd.read_json('data/world_bank_projects.json')
df['mjtheme_namecode2'] = range(len(df))  #add new column for new json string with '' names filled in

for x in range(0,len(df)) :
    codes_normalized = json_normalize(df.ix[x,'mjtheme_namecode'])
    for y in range(0,len(codes_normalized)) : 
        if (codes_normalized.ix[y,'code'] == '1') : codes_normalized.name[y] = 'Economic management'
        if (codes_normalized.ix[y,'code'] == '2') : codes_normalized.name[y] = 'Public sector governance'
        if (codes_normalized.ix[y,'code'] == '3') : codes_normalized.name[y] = 'Rule of law'
        if (codes_normalized.ix[y,'code'] == '4') : codes_normalized.name[y] = 'Financial and private sector development'
        if (codes_normalized.ix[y,'code'] == '5') : codes_normalized.name[y] = 'Trade and integration'            
        if (codes_normalized.ix[y,'code'] == '6') : codes_normalized.name[y] = 'Social protection and risk management'
        if (codes_normalized.ix[y,'code'] == '7') : codes_normalized.name[y] = 'Social dev/gender/inclusion'            
        if (codes_normalized.ix[y,'code'] == '8') : codes_normalized.name[y] = 'Human development'            
        if (codes_normalized.ix[y,'code'] == '9') : codes_normalized.name[y] = 'Urban development'            
        if (codes_normalized.ix[y,'code'] == '10') : codes_normalized.name[y] = 'Rural development'            
        if (codes_normalized.ix[y,'code'] == '11') : codes_normalized.name[y] = 'Environment and natural resources management'            
    df.ix[x,'mjtheme_namecode2'] = codes_normalized.to_json(orient='records') # convert back to json string and populate new column

# example of corrected string
df.ix[209,'mjtheme_namecode2']  #show example of filled in strings in place of ''
'[{"code":"2","name":"Public sector governance"},{"code":"6","name":"Social protection and risk management"}]'

# same string before it was corrected
df.ix[209,'mjtheme_namecode']
[{'code': '2', 'name': 'Public sector governance'}, {'code': '6', 'name': ''}]




