


OVERVIEW

With sudden increase in original film production in mega companies, Microsoft wants to get an in-depth understanding of the movie industries to help her determine if it’s a business they can venture. I used the return on investment as a measure to gauge the profitability of particular movies, looked into most popular publisher and rating and also used production budget against domestic gross to help determine the correction of the two output and also how production budget influence movie output in term of sales.


BUSINESS PROBLEM

The potential problem that microsoft has is basically to determine if they should venture into the movie industry and just get to basically understand the key determiners of the market that influence good outcome in term of sale and basically helping them understand the success of a movie is basically determine by not only common factor like gross income but things also like the publisher so Microsoft has to position itself well to beat key players like Amazon and Netflix. I used these key analysis to help them understand the problem 1.Original language 2.ROI and Production Budget 3.Publishers and Rating It's important for Microsoft to carefully consider both the ROI potential and the publishers they work with in order to make informed decisions about their venture into original movie content.


DATA UNDERSTANDING

The potential problem that microsoft has is basically to determine if they should venture into the movie industry and just get to basically understand the key determiners of the market that influence good outcome in term of sale and success of a movie is basically determine by not only common factor like gross income but things also like the publisher so Microsoft has to position itself well to beat key players like Amazon and Netflix.I used below key analysis to help them understand the problem 1.Return On Investement(ROI) i will get this analyses from the movie budget dataset 2.Production Budget 3.Publishers and Ratings It's important for Microsoft to carefully consider both the ROI potential and the publishers they work with in order to make informed decisions about their venture into original movie content.

#imported the necessary tool for data cleaning and analyses.


import pandas as pd

import sqlite3

import csv

import numpy as np

import seaborn as sns

import matplotlib.pyplot as plt

%matplotlib inline

#Ignore warnings

import warnings

warnings.filterwarnings('ignore')

1.Get to answer original language influence on the movie views


#import TMDB movie dataset



movies =pd.read_csv('tmdb.movies.csv.gz',index_col = 0)


movies


#having gotten the data,an overview of the columns shows the following (genre id,id,original language,original title,popularity,release date,title,vote average and vote count).I will clean the above data and get to get more insight on the original language and views based on the production language.

#get to understanding datatype i will be using


movies.info()


#get to see statistical ananlysis of the data

movies.describe()


#get to see if there is null values in the data


movies.isnull().sum()



#get to see the key values in the dataset


movies.keys()


#get to create an histogram for the vote count column to have an insight of how it is distributed

movies['vote_count'].hist()



#get to get in the vote count column those above 20000

movies[movies['vote_count']>20000]




#the correlation btw vote_average and vote_count is weak positive correlation

movies_corr =movies[['vote_average', 'vote_count']].corr()

movies_corr


	vote_average 	vote_count
vote_average 	1.00000 	0.08637
vote_count 	0.08637 	1.00000

#The seaborn graph assist to clearly show the correlation of the two and as per the graph its a weak as it is below 0.2

sns.heatmap(movies_corr, cmap ='coolwarm', annot =True)


#another insight is the relation of the movies and the languages used and mostly the audience prefer english as shown below

movies['original_language'].value_counts()



#the correlation btw vote_average and popularity is weak positive correlation

movies_pop =movies[['popularity', 'vote_average']].corr()

movies_pop

	popularity 	vote_average
popularity 	1.000000 	0.065273
vote_average 	0.065273 	1.000000

#create a scatterplot for the vote average and popularity 



2.Analyse the TN dataset to get more insight on ROI

#import movie budget dataset

movie_budgets =pd.read_csv('tn.movie_budgets.csv.gz')



#get to see the statistical analyses of the data like mean 

movie_budgets.describe()


#get to see datatype 

movie_budgets.dtypes



#get to see if the data has null values

movie_budgets.isnull().any()

id                   False
release_date         False
movie                False
production_budget    False
domestic_gross       False
worldwide_gross      False
dtype: bool

#movie_budgets_corr =movie_budgets[['domestic_gross','foreign_gross']].corr()

#movie_budgets_corr

#get to remove the $ and '' values in the data to help do further statistical analyses in the data

movie_budgets['domestic_gross'] = pd.to_numeric(movie_budgets['domestic_gross'].str[1:].str.replace(',', ''))

movie_budgets['production_budget'] =pd.to_numeric(movie_budgets['production_budget'].str[1:].str.replace(',', ''))

movie_budgets['worldwide_gross'] = pd.to_numeric(movie_budgets['worldwide_gross'].str[1:].str.replace(',', ''))

movie_budgets

​


#get to calculate the ROI by substracting production budget from the domestic gross

#i have calculated the return of investment based on the difference of domestic gross and production budget

movie_budgets['ROI'] = movie_budgets['domestic_gross'] - movie_budgets['production_budget']

movie_budgets


#create a scatterplot 

sns.scatterplot(data=movie_budgets,x='production_budget', y='domestic_gross')

plt.title('production_budget vs domestic_gross')



# Compute the correlation matrix

corr = movie_budgets[['production_budget', 'domestic_gross', 'worldwide_gross', 'ROI']].corr()

​

# Create heatmap of the correlation matrix

sns.heatmap(corr, annot=True, cmap='coolwarm')

plt.title('Correlation Matrix')

plt.show()

# Get the movie with the highest ROI

highest_ROI_movie = movie_budgets.loc[movie_budgets['ROI'].idxmax(), 'movie']

print(f"Highest ROI movie: {highest_ROI_movie}")

Highest ROI movie: Star Wars Ep. VII: The Force Awakens

# Get the movie with the highest domestic gross

highest_domestic_gross_movie = movie_budgets.loc[movie_budgets['domestic_gross'].idxmax(), 'movie']

print(f"Highest domestic gross movie: {highest_domestic_gross_movie}")

Highest domestic gross movie: Star Wars Ep. VII: The Force Awakens

#this two previous finds supports my correlation matrix on the correction between ROI and Domestic_gross that had a strong positive correlation in that as domestic gross increase ROI increases.

highest_production_budget_movie = movie_budgets.loc[movie_budgets['production_budget'].idxmax(), 'movie']

print(f"Highestproduction budget movie: {highest_production_budget_movie}")

Highestproduction budget movie: Avatar

#avatar had the highest prouction budget meaning production budget doesnt mean more profitability

#Determine the movies that made losses in terms of ROI

# Filter the rows where ROI is negative

negative_ROI_movies = movie_budgets.loc[movie_budgets['ROI'] < 0, 'movie']

​

# Print the movies with negative ROI

print(negative_ROI_movies)

1       Pirates of the Caribbean: On Stranger Tides
2                                      Dark Phoenix
8                                    Justice League
9                                           Spectre
11                          Solo: A Star Wars Story
                           ...                     
5772                                      Newlyweds
5776                                The Mongol King
5777                                         Red 11
5779                  Return to the Land of Wonders
5780                           A Plague So Pleasant
Name: movie, Length: 3105, dtype: object

# Sort the DataFrame by ROI in descending order

sorted_movie = movie_budgets.sort_values('ROI', ascending=False)

​

# Get the top 10 movies with the highest ROI

top_10_ROI_movies = sorted_movie.head(10)

​

# Display the top 10 movies

print(top_10_ROI_movies[['movie', 'ROI']])

# Create a horizontal bar chart of the top 10 movies with the highest ROI

sns.set_style('whitegrid')

sns.barplot(data=top_10_ROI_movies, x='ROI', y='movie', color='green')

plt.xlabel('ROI')

plt.ylabel('Movie Title')

plt.title('Top 10 Movies with the Highest ROI')

​

# Display the plot

plt.show()

Identify the movies with highest ROI for the last 10 years

#identify the movies with highest ROI for the last 10 years having resetted the release year as the index

#reset the release_date column as a datetime and set it as the index column

# Convert the release_date column to datetime and set it as index

movie_budgets['release_date'] = pd.to_datetime(movie_budgets['release_date'])

movie_budgets.set_index('release_date', inplace=True)

​

# Filter the DataFrame to only include the rows from the past 10 years

ten_years_ago = pd.to_datetime('today') - pd.DateOffset(years=10)

filtered_df = movie_budgets.loc[movie_budgets.index >= ten_years_ago]

​

# Sort the DataFrame by ROI in descending order and retrieve the top 10 movies

sorted_df = filtered_df.sort_values('ROI', ascending=False)

top_10_ROI_movies = sorted_df.head(10)

​

# Print out the top 10 movies with their ROI values

print(top_10_ROI_movies[['movie', 'ROI']])



# Create a bar plot of the top 10 movies by ROI

sns.set_style('darkgrid')

plt.figure(figsize=(10, 6))

sns.barplot(x='movie', y='ROI', data=top_10_ROI_movies)

plt.title('Top 10 Movies by ROI in the Past 10 Years')

plt.xlabel('Movie')

plt.ylabel('ROI')

plt.xticks(rotation=45, ha='right')

plt.tight_layout()

plt.show()

​

3.Analyse the review dataset to get an insight of the publisher and rating

review = pd.read_csv('reviews.tsv' ,sep ='\t',encoding = 'latin')

review

​

review['publisher'].unique()

array(['Patrick Nabarro', 'io9.com', 'Stream on Demand', ...,
       'The Big Issue (Australia)', 'The Jacobin', 'OZY'], dtype=object)

review['rating'].unique()


#The ratings appear to be in different formats

#such as letter grades (A+, A, B-, etc.)

# percentages (e.g. 80%)

#fractions (e.g. 3/5), and

#even non-numeric characters (such as 'N' and 'T').

​

#create a function that takes each rating as an input

#checks its format, 

#converts it to a numerical rating on a scale of 1 to 10. 

def convert_rating(rating):

    try:

        if '/' in rating:

            numer, denom = rating.split('/')

            return round(int(numer)/int(denom)*10, 1)

        elif '-' in rating:

            left, right = rating.split('-')

            return round((int(left) + int(right))/2, 1)

        elif rating in ['N', 'R', 'T']:

            return None

        else:

            return float(rating)

    except:

        return None

#apply this function to each element in the ratings column using the apply() method.

# create a new column showing the ratings in numeric.

# Identify the publisher who had the highest rating.

review['numeric_rating'] = review['rating'].apply(convert_rating)

review

54432 rows × 9 columns

#calculate the highest rated publisher

highest_rated_publisher = review.groupby('publisher')['numeric_rating'].mean().sort_values(ascending=False).index[0]

print("The highest rated publisher is:", highest_rated_publisher)

The highest rated publisher is: FilmFour.com

#get the top ten publishers

top_ten_publishers = review.groupby('publisher')['numeric_rating'].mean().sort_values(ascending=False).head(10)

print(top_ten_publishers)

publisher
FilmFour.com                               10.0
Filmspotting                               10.0
Daily Journal (Kankakee, IL)               10.0
Black Star News                            10.0
Providence American                        10.0
Martha's Vineyard Times (Massachusetts)    10.0
FF2 Media                                  10.0
Sly Fox                                    10.0
Attitude                                   10.0
Afro-American                              10.0
Name: numeric_rating, dtype: float64

# Explore fresh distribution 

sns.catplot(x="fresh", kind="count", data=review)

<seaborn.axisgrid.FacetGrid at 0x22b801aaaf0>

#The dataset has more fresh movies as compared to rotten. 

#movies with a score above 70% are considered to be fresh while movies with a score of 59% or lower as considered to as rotten

CONCLUSIONS

The three key factor I used for analysis show that ROI is important and this is basically determined by production cost against domestic gross and also the language of the movie will influence production and English is the most preferred movie type as it attracts more audience hence mostly watched and lastly publishers and their rating is a key factor as they will assist push for sales and good rating. Top known publisher in the industry plays a key role in pushing for movies sales.


RECOMMENDATION

Consider investing in movies with higher production budgets: Since there is a positive correlation between production budget and domestic gross, investing in movies with higher production budgets may be more likely to yield higher returns. However, it's important to keep in mind that higher production budgets also come with higher risks.

Look for movies with good ratings: Movies with good ratings, especially those from top publishers, may be more likely to attract audiences and generate higher ticket sales. Investing in movies with good ratings can help minimize the risk of a box office flop.

Diversify your investments: While there is a positive correlation between production budget and domestic gross, it's important to diversify your investments to minimize risk. Investing in a variety of movies with different production budgets, genres, and ratings can help spread out the risk and increase the likelihood of a successful investment portfolio.

Consider the overall market trends: It's important to consider the overall market trends and consumer preferences when making investment decisions in the movie industry. For example, if streaming services are becoming more popular, it may be wise to invest in movies that have the potential to perform well on streaming platforms. Keeping up with the latest market trends can help you make informed investment decisions.
