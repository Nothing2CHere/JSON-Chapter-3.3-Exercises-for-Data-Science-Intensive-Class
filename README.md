# JSON-Chapter-3.3-Exercises-for-Data-Science-Intensive-Class


# Find the 10 countries with most projects
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





# Find the top 10 major project themes (using column 'mjtheme_namecode')
df = pd.read_json('data/world_bank_projects.json')

all_codes = df.mjtheme_namecode[0]    # extract out just the json column
for x in range(1,len(df)) :           # combine all the json rows together
    all_codes = all_codes + df.mjtheme_namecode[x]  
    
all_codes_normalized = json_normalize(all_codes).sort_values(['code','name']) # normalize all together and sort
all_codes_normalized.name = all_codes_normalized.name.replace('', np.nan, regex=True).fillna(method='bfill') # replace empty strings and backfill correct strings
all_codes_normalized.groupby(['code','name']).size().sort_values(ascending=False).head(100)  #groupby

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



