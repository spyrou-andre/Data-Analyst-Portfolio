


import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt


#Reads the excell file into a dataframe 
Appartments_Flats = pd.read_csv (r'C:\Users\ASpyrou\PycharmProjects\pythonProject\pythonProject\vamos\vamosela\vamosela\apartments_flats.csv' )
Houses = pd.read_csv (r'C:\Users\ASpyrou\PycharmProjects\pythonProject\pythonProject\vamos\vamosela\vamosela\houses.csv' )

#--------------------------------------------------------------------------------------------------------
#Data Information
#--------------------------------------------------------------------------------------------------------

#Checking for NaN values
Appartments_Flats.isnull().sum()
Houses.isnull().sum()

#Dataframe Information
Appartments_Flats.info()
Houses.isnull().info()


#--------------------------------------------------------------------------------------------------------


#Makes all capial cases letter to lower case
Appartments_Flats['Title'] = Appartments_Flats['Title'].str.lower()
#--------------------------------------------------------------------------------------------------------
Houses['Title'] = Houses['Title'].str.lower()


#Concatenating all titles into a single string.
Title_str = Appartments_Flats.Title.str.cat()
#--------------------------------------------------------------------------------------------------------
Title_str_H = Houses.Title.str.cat()


#Splitting tthe titles into words
Title_str1 = pd.DataFrame()
Title_str1['Word'] =Title_str.split()
#--------------------------------------------------------------------------------------------------------
Title_str2 = pd.DataFrame()
Title_str2['Word'] =Title_str.split()


#Creating a New Dataframe that shows the count of each word in the title and sorting them by highest first
Title_Wordcount = Title_str1.groupby(['Word']).size().reset_index(name="Word Count").sort_values(["Word Count"],ascending=[False])
#--------------------------------------------------------------------------------------------------------
Title_Wordcount_H = Title_str2.groupby(['Word']).size().reset_index(name="Word Count").sort_values(["Word Count"],ascending=[False])


#Creating a loop that identifies the number of bedrooms in the title
for n in map(str,list(range(1, 11))):
    Appartments_Flats.loc[(Appartments_Flats['Title'].str.contains(n)) , 'Bedrooms_No'] = n+'-bedroom'
#--------------------------------------------------------------------------------------------------------    
for n in map(str,list(range(1, 11))):
    Houses.loc[(Houses['Title'].str.contains(n)) , 'Bedrooms_No'] = n+'-bedroom'    


#Checking for null values
Appartments_Flats['Bedrooms_No'] .isnull().sum()
#--------------------------------------------------------------------------------------------------------
Houses['Bedrooms_No'] .isnull().sum()


#Creating a dataframe based on null values of Bedrooms No
AppNullVals = Appartments_Flats.loc[Appartments_Flats['Bedrooms_No'] .isnull()]
#--------------------------------------------------------------------------------------------------------
HouNullVals = Houses.loc[Houses['Bedrooms_No'] .isnull()]


#Creating a column to identify if there is the word "Studio" in the title
AppNullVals.loc[(AppNullVals['Title'].str.contains('studio')) , 'Studio'] ="Yes"  
#--------------------------------------------------------------------------------------------------------
HouNullVals.loc[(HouNullVals['Title'].str.contains('studio')) , 'Studio'] ="Yes"  


#Showing the number of listings that have null in bedrooms No and Yes or No if they containt the word Studio 
AppNullVals.groupby(['Studio']).size()
#--------------------------------------------------------------------------------------------------------
HouNullVals.groupby(['Studio']).size()


#Replacing nan values of bedrooms no with 1 these are the studions
Appartments_Flats['Bedrooms_No'] = Appartments_Flats['Bedrooms_No'].fillna('1-bedroom') .astype(str) 
#--------------------------------------------------------------------------------------------------------   
Houses['Bedrooms_No'] = Houses['Bedrooms_No'].fillna('1-bedroom') .astype(str) 


#Splitting the Location into District and Area
Appartments_Flats[['District','Area']]=Appartments_Flats['Location'].str.split(',', expand = True)
#--------------------------------------------------------------------------------------------------------
Houses[['District','Area']]=Houses['Location'].str.split(',', expand = True)


#Count the number of Listings per District
District_Count = Appartments_Flats.groupby(['District']).size().reset_index(name="District_Count")
#--------------------------------------------------------------------------------------------------------
District_Count_H = Houses.groupby(['District']).size().reset_index(name="District_Count")


#Number of Listings per disrict per Number of Bedrooms
District_Listings = Appartments_Flats.groupby(['District','Bedrooms_No']).size().reset_index(name="Listings_Number").sort_values(["Bedrooms_No","District"],ascending=[True,True])
#--------------------------------------------------------------------------------------------------------
District_Listings_H = Houses.groupby(['District','Bedrooms_No']).size().reset_index(name="Listings_Number").sort_values(["Bedrooms_No","District"],ascending=[True,True])


# set plot style: grey grid in the background:
sns.set(style="darkgrid")
# Set the figure size
plt.figure(figsize=(16, 8))


#Plot that shows the number of listings per bedroom No per District
sns.barplot(x="District", y="Listings_Number", hue="Bedrooms_No", data=District_Listings, ci=None); plt.title('Appartments Listings No per Bedroom No per District')
#------------------------------------------------------------------------------------------------------------------------------------------------------------
sns.barplot(x="District", y="Listings_Number", hue="Bedrooms_No", data=District_Listings_H, ci=None); plt.title('Houses Listings No per Bedroom No per District')


#Dataset for calculating the average price per districr per bedroom no
BedroomPrices = Appartments_Flats[['Bedrooms_No','District','Price']]
#--------------------------------------------------------------------------------------------------------
BedroomPrices_H = Houses[['Bedrooms_No','District','Price']]


#Average price calculation per District per Number of Bedrooms
Average_Price = round((District_Listings.merge(BedroomPrices, left_on=['District','Bedrooms_No'], right_on=['District','Bedrooms_No'])
         .groupby(['District','Bedrooms_No'], as_index=False)['Price']
         .mean()))
#--------------------------------------------------------------------------------------------------------
Average_Price_H = round((District_Listings_H.merge(BedroomPrices_H, left_on=['District','Bedrooms_No'], right_on=['District','Bedrooms_No'])
         .groupby(['District','Bedrooms_No'], as_index=False)['Price']
         .mean()))


#Renaming columns
Average_Price.rename(columns = {'Price':'Average_Price'}, inplace = True)
#--------------------------------------------------------------------------------------------------------
Average_Price_H.rename(columns = {'Price':'Average_Price'}, inplace = True)


#Adding the average price to the dataframe 
District_Listings = District_Listings.merge(Average_Price,on=['District','Bedrooms_No'],how='inner') 
#--------------------------------------------------------------------------------------------------------
District_Listings_H = District_Listings_H.merge(Average_Price_H,on=['District','Bedrooms_No'],how='inner') 


#------------------------------------------------------------------------------------------------------------------------------------------------------------
#------------------------------------------------------------------------------------------------------------------------------------------------------------
#Appartments
#------------------------------------------------------------------------------------------------------------------------------------------------------------

#Creating two datasets that are based for 1-3 bedrooms and 4-6 bedrooms
One_To_Four_Beds = District_Listings.loc[(District_Listings.Bedrooms_No == '1-bedroom') | (District_Listings.Bedrooms_No == '2-bedroom') | (District_Listings.Bedrooms_No == '3-bedroom') | (District_Listings.Bedrooms_No == '4-bedroom')].sort_values(["Bedrooms_No"],ascending=[True])


#Plots two graphs in one figure they have different axes and the scientific format is removed
figure, ax = plt.subplots(1, 2,  figsize=(16,8)) ; plt.ticklabel_format(style='plain', axis='y') 


#Plots two graphs in one figure they have different axes and the scientific format is removed
figure, ax = plt.subplots(1, 2,  figsize=(16,8)) 
figure.suptitle(' Count and Average prices for Appartmnets per bedroom No per District')
sns.barplot(x="Bedrooms_No", y="Listings_Number", hue="District", data=One_To_Four_Beds, ci=None, ax=ax[0]);ax[0].set_title('Listings Count')
sns.barplot(x="Bedrooms_No", y="Average_Price", hue="District", data=One_To_Four_Beds, ci=None, ax=ax[1]);ax[1].set_title('Average Price (EUR)')

# Format the tick labels for the first subplot
ax[0].set_yticklabels([f"{y:,.0f}" for y in ax[0].get_yticks()])
ax[1].set_yticklabels([f"{y:,.0f}" for y in ax[1].get_yticks()])

#------------------------------------------------------------------------------------------------------------------------------------------------------------
#------------------------------------------------------------------------------------------------------------------------------------------------------------
#Houses
#------------------------------------------------------------------------------------------------------------------------------------------------------------

#Creating two datasets that are based for 1-3 bedrooms and 4-6 bedrooms
Two_To_Six_Beds_H = District_Listings_H.loc[(District_Listings_H.Bedrooms_No == '2-bedroom') | (District_Listings_H.Bedrooms_No == '3-bedroom') | (District_Listings_H.Bedrooms_No == '4-bedroom') | (District_Listings_H.Bedrooms_No == '5-bedroom') | (District_Listings_H.Bedrooms_No == '6-bedroom') ].sort_values(["Bedrooms_No"],ascending=[True])

#Plots two graphs in one figure they have different axes and the scientific format is removed
figure, ax = plt.subplots(1, 2,  figsize=(16,8)) ; plt.ticklabel_format(style='plain', axis='y') 


#Plots two graphs in one figure they have different axes and the scientific format is removed
figure, ax = plt.subplots(1, 2,  figsize=(16,8)) 
figure.suptitle(' Count and Average prices for Houses per bedroom No per District')
sns.barplot(x="Bedrooms_No", y="Listings_Number", hue="District", data=Two_To_Six_Beds_H, ci=None, ax=ax[0]);ax[0].set_title('Listings Count')
sns.barplot(x="Bedrooms_No", y="Average_Price", hue="District", data=Two_To_Six_Beds_H, ci=None, ax=ax[1]);ax[1].set_title('Average Price (EUR)')

# Format the tick labels for the first subplot
ax[0].set_yticklabels([f"{y:,.0f}" for y in ax[0].get_yticks()])
ax[1].set_yticklabels([f"{y:,.0f}" for y in ax[1].get_yticks()])

#------------------------------------------------------------------------------------------------------------------------------------------------------------
#------------------------------------------------------------------------------------------------------------------------------------------------------------

#Comparison

#Plots two graphs in one figure they have different axes and the scientific format is removed
figure, ax = plt.subplots(2, 2,  figsize=(16,8)) ; plt.ticklabel_format(style='plain', axis='y') 


sns.barplot(x="Bedrooms_No", y="Listings_Number", hue="District", data=One_To_Four_Beds, ci=None, ax=ax[0][0]);ax[0][0].set_title('Apps. Listings Count')
sns.barplot(x="Bedrooms_No", y="Average_Price", hue="District", data=One_To_Four_Beds, ci=None, ax=ax[0][1]);ax[0][1].set_title('Apps. Average Price (EUR)')
sns.barplot(x="Bedrooms_No", y="Listings_Number", hue="District", data=Two_To_Six_Beds_H, ci=None, ax=ax[1][0]);ax[1][0].set_title('House Listings Count')
sns.barplot(x="Bedrooms_No", y="Average_Price", hue="District", data=Two_To_Six_Beds_H, ci=None, ax=ax[1][1]);ax[1][1].set_title('House Average Price (EUR)')

# Format the tick labels for the first subplot
ax[0][0].set_yticklabels([f"{y:,.0f}" for y in ax[0][0].get_yticks()])
ax[0][1].set_yticklabels([f"{y:,.0f}" for y in ax[0][1].get_yticks()])
ax[1][0].set_yticklabels([f"{y:,.0f}" for y in ax[1][0].get_yticks()])
ax[1][1].set_yticklabels([f"{y:,.0f}" for y in ax[1][1].get_yticks()])

# remove axis labels
ax[0][0].set_xlabel('')
ax[0][1].set_xlabel('')



