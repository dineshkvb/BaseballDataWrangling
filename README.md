# BaseballDataWrangling


# coding: utf-8

# # 1)	Load in the appropriate csv file as a pandas dataframe (batting.csv)

# In[2]:


import numpy as np

import pandas as pd
from IPython.display import display

df = pd.read_csv("batting.csv")
pd.options.display.max_columns = None


# In[463]:


df.head(3)


# # 2)	Print out the dimensions and info about the dataframe you just created

# In[464]:


df.info()


# In[465]:


df.shape


# # 3)	How many players have hit 40 or more HRs in one single season? (Number only)

# In[466]:


df["playerID"][df["HR"]>=40].count()


#  # 4)	How many players have hit more than 600 HRs for their career? (Dataframe)

# In[467]:


df1=df.groupby('playerID').sum()


# In[469]:


df1[(df1['HR']>600)]


# # 5)	How many players have hit 40 2Bs, 10 3Bs, 200 Hits, and 30 HRs (inclusive) in one season? (Number Only)

# In[470]:


df2 = df[(df['2B']>=40) & (df['3B']>=10) & (df['H']>=200) & (df['HR']>=30) ]


# In[471]:


df2['playerID'].count()


# # 6)	How many players have had 100 or more SBs in a season? (Dataframe)

# In[472]:


df[df["SB"]>=100].head()


# # 7)	How many players in the 1960s have hit more than 200 HRs? (Dataframe)

# In[473]:


df1 = df[(df["yearID"].between(1960, 1969, inclusive=True))].head()


# In[474]:


df2=df.groupby('playerID').sum()


# In[475]:


df2[(df2['HR']>200)].head()


# # 8)	Who has hit the most HRs in history? (Dataframe)

# In[477]:


df3=df.groupby('playerID').sum()


# In[478]:


df3[df3['HR'] == (df3['HR'].max())]


# # 9)	Who had the most hits in the 1970s? (Dataframe)

# In[479]:


df4 = df[(df["yearID"].between(1970, 1979, inclusive=True))].head()


# In[480]:


df5=df4.groupby('playerID').sum()


# In[481]:


df5[df5['H'] == (df5['H'].max())]


# # 10)	Top 10 highest OBP (on base percentage) with at least 500 PAs in 1977? (Sorted by hits)  (Dataframe)

# In[482]:


df['OBP'] = (df['H'] + df['BB'] + df['HBP']) / (df['AB'] + df['BB'] + df['HBP'] + df['SF'])


# In[483]:


df['PA'] = (df['AB'] + df['BB'] + df['IBB'] + df['SF'] + df['SH'])


# In[484]:


t1=df[(df["PA"]>=500) & (df['yearID'] == 1997)]


# In[485]:


t2 = (t1.sort_values('OBP', ascending = False)).head(10)


# In[486]:


t2.sort_values('R', ascending = False)


# # 11)	Top 8 highest averages in 2013 with at least 300 PAs? (Dataframe)

# In[487]:


df['AVG'] = (df['H']) / (df['AB'])


# In[488]:


t3 = df[(df['PA']>=300) & (df['yearID'] == 2013)]


# In[489]:


(t3.sort_values('AVG', ascending = False)).head(8)


# # 12)	Leaders in hits from 1940 up to and including 1949. (Dataframe)

# In[490]:


t4 = df[(df["yearID"].between(1940, 1949, inclusive=True))]


# In[491]:


(t4.sort_values('H', ascending = False)).head(10)


# # 13)	Who led MLB(entire dataset) with the most hits the most times?  And how many times?  (Dataframe, Number)

# In[10]:


BD = pd.read_csv("Batting.csv").fillna(0)


# In[11]:


tmp1 = BD.groupby(['playerID','yearID'],as_index=False)["HR"].sum()


# In[12]:


tmp1 = tmp1.sort_values(by="HR",ascending=False)


# In[13]:


tmp1 = tmp1.groupby('yearID',as_index=False).first()


# In[14]:


tmp1 = tmp1.groupby('playerID',as_index=False).count()


# In[15]:


tmp1 = tmp1.sort_values(by="HR",ascending=False)


# In[18]:


tmp1 = tmp1.rename(columns={"playerID":"playerID","yearID":"NumTimes"})


# In[17]:


tmp1[["playerID","NumTimes"]].head(10).reset_index()


# # 14) Which players have played the most games for their careers?  Top 5, descending by games played presented as a dataframe

# In[493]:


(df.groupby('playerID').sum()).sort_values('G', ascending=False).head(5)


# # 15) How many players have had more than 3000 hits for their careers while also hitting 500 or more HRs?  Just a number is okay here

# In[297]:


t6= (df.groupby('playerID').sum())


# In[307]:


t5 = t6[(t6['H']>3000) & (t6['HR'] >= 500)]


# In[308]:


t5


# # 16) How many HRs were hit during the entire 1988 season?  Just a number is okay here

# In[315]:


t7=df[(df['yearID'] == 1988)]


# In[317]:


t7[('HR')].sum()


# # 17) Please filter out and show me the top 3 average seasons by Wade Boggs during his career in seasons in which he had at least 500 ABs.  I would like a dataframe sorted by average.

# In[325]:


df['playerName'] = df['nameFirst'] + ' ' + df['nameLast']


# In[330]:


t8=df[(df['playerName'] == 'Wade Boggs') & (df['AB']>=500)]


# In[333]:


t8.sort_values('AVG', ascending = False).head(3)


# # 18) Please filter out the top OBPs for the 1995 season with at least 400 PAs, sorted by OBP.  I would like a dataframe for this

# In[338]:


t9=df[(df['yearID'] == 1995) & (df['PA']>=400)]


# In[339]:


t9.sort_values('OBP', ascending = False).head(5)


# # 19) Who had the most 3Bs (in total) in 1922, 1925, 1926, and 1928?  I would like a dataframe with just the leader

# In[341]:


t10=df[(df['yearID'].isin([1922,1925,1926,1928]))]


# In[346]:


t11=(t10.groupby('playerID').sum())


# In[350]:


t12 = t11[(t11['3B']==t11['3B'].max())]


# In[351]:


t12


# # 20) How many players have hit 30 or more HRs in season while also stealing (SB) 30 more or bases?  A number is okay here

# In[353]:


t13=df[(df['HR'] >= 30) & (df['SB']>=30)]


# In[355]:


t13['playerID'].count()


# # 21) Who had the highest OBP is 1986 with at least 400 PAs? (Dataframe)

# In[362]:


t14=df[(df['yearID'] == 1986) & (df['PA']>=400)]


# In[363]:


t15 = t14[(t14['OBP']==t14['OBP'].max())]


# In[364]:


t15


# # 22) Same question but for 1997 and only in the NL (check league ID)? (Dataframe)

# In[366]:


t16=df[(df['yearID'] == 1997) & (df['PA']>=400) & (df['lgID']=='NL')]


# In[368]:


t17 = t16[(t16['OBP']==t16['OBP'].max())]


# In[369]:


t17


# # 23) Who had more than the league average HRs in 2012 (filter out all players with less 500 PAs)? (Dataframe)

# In[371]:


t18=df[(df['yearID'] == 2012) & (df['PA']>=500)]


# In[374]:


t19 = t18[(t18['HR']> t18['HR'].mean())]


# In[375]:


t19


# # 24) Who is the youngest player to hit 50 or more HRs in a single season? (Dataframe)

# In[390]:


df["Age"]= (df['yearID'] - df['birthYear'])


# In[391]:


t20=df[(df['HR']>=50)]


# In[392]:


t21= t20[(t20['Age'] == t20['Age'].min())]


# In[393]:


t21


# # 25) Who are the five youngest players to hit 300 or more HRs for their career? (Dataframe)

# In[40]:


import datetime

df['AgeRightNow']=datetime.datetime.now().year-df.birthYear


# In[41]:


t22 = df.groupby(["playerID"],as_index=False).agg({"HR":"sum","AgeRightNow":'first',"birthYear":'first'})


# In[42]:


t23 = t22.loc[t22["HR"]>=300]


# In[43]:


t23 = t23.sort_values(by='AgeRightNow',ascending=True).head(5)
t23.reset_index()


# # BONUS:  Graph total HRs per season using bar graph

# In[440]:


import matplotlib.pyplot as plt
get_ipython().magic('matplotlib inline')
import seaborn as sns
get_ipython().magic('matplotlib inline')


# In[460]:


df.pivot_table(values='HR',index='yearID', aggfunc=np.sum).plot.bar(figsize=(30,30),lw=3)


# # Using a line graph please graph the average HRs per AB (think about this) per season

# In[459]:


df.pivot_table(values='HR',index='AB', aggfunc=np.mean).plot.line(figsize=(20,30),lw=2)

