# Airbnb-Seattle 2016
This repository shows analysis of airbnb Seattle data

## Business Understanding:
**What is Airbnb?**  
Airbnb is an online marketplace connecting travelers with local hosts. On one side, the platform enables people to list their available space and earn extra income in the form of rent. On the other, Airbnb enables travelers to book unique home stays from local hosts, saving them money and giving them a chance to interact with locals. Catering to the on demand travel industry, Airbnb is present in over 190 countries across the world.

**How Airbnb works?**  
Hosts list out their property details on Airbnb along with other factors like pricing, amenities provided etc. **(Listings)**  
Travellers search for a property in the city where they wish to stay and browse available options according to price, amenities etc. **(Price,Amenities)**  
Booking is made through Airbnb where traveller pays the amount mentioned by host and some additional money as transaction charges.  
Host approves the booking. Traveller stays there and finally Airbnb pays the amount to the host after deducting their commission.  
The host and the traveler can rate each other and can write reviews based on the experience. **(Review Model)**  

**What are the key customer segments defining Airbnb’s Business Model?**  
1. Hosts - Property owners  
2. Guests - Mostly Travelers, renters  


**What makes people use Airbnb?**   
Below are the few reasons people use airbnb, both Hosts and Guests.  
Enables owners to list their space on the platform and earn rental money. By becoming hosts they can earn money.  
Airbnb provides insurance to listed properties. Safety for the property owners.  
Gives cheap options to travellers to stay with local hosts. Guests like it since most of the time they would get a better deal than staying at hotel.  
Facilitates the process of booking living space for travelers.   
Rating and review system for hosts and guests.  
User-friendly app and web based structure.  

**Revenue model: How Airbnb makes money?**  
Airbnb offers free listings to property owners and let’s travellers browse the listed spaces and select the one which best suits their needs on the platform. The business model of Airbnb is such that the booking and monetary transactions are done on Airbnb’s platform. This is from where the company earns its share of revenue from 2 different sources which have been explained below:

Commission from Property Owners (Hosts)  
Airbnb charges flat **10%** commission from hosts upon every booking done through the platform.

Transaction fee from Travellers (Guests)  
Airbnb charges **3%** of the booking amount as transaction charges from travellers upon every confirmed booking.


**Key problems and Solutions:**  
**1.Trust Problem:** 
The biggest problem faced by travelers or hosts using Airbnb is the trust factor. After all giving your space to a stranger as a host and living with strangers at their place as a traveller might not be easy. But the verification process is in place for every host and traveler on its platform. Apart from the verification badge, Airbnb also motivates people to sign up with their Facebook account or at least link it with their account for better transparency.

This is not all. In case something goes wrong, an insurance policy is available too.

**2.Traveler Retention:** 
Another problem being faced is the retention problem. In order to grow, the company needs to retain its travellers so that they do not choose a hotel on their next vacations. In order to retain them, it gives offers, promotional codes and credits to frequent travelers. As a solution to this problem, it also sends such promotions to hosts as to motivate them to take a vacation and stay in an Airbnb at their favourite destination.

**The Future of Airbnb:**  
Airbnb is already a multi billion dollar company and is sure to grow further. Having a presence in 190+ countries across the world, it is now concentrating to further increase the daily transactions on its platform. The unique business model of Airbnb has become stronger as people prefer staying at an Airbnb inn rather than a hotel.

Considering involved parties, Hosts and Guests, 
I wanted to find answers to below questions from analysing airbnb 2016 seattle data

1. What are the primary factors affect the listing price?
2. What are the important features helps owner in becoming super host?
3. Which area in Seattle is most expensive to rent?
4. Is there any seasonal effect in Seattle airbnb listing price? What would be best time to visit if I want to save some money?
5. Are there any homes in 2016 that never occupied? Is there any significant factors in why these places were never rented?

This analysis would be worth considering before renting and listing any property on Airbnb.


## Data Understanding:
I am using airbnb 2016 seattle data. We have 3 data csv files with 

**1. listings.csv**  
This Data set has all the information about the 2016 Seattle city airbnb listings. The shape of this data is ` 3818 records and 92 columns `.
Considering the problem we are trying to solve and the analysis I have selected below features 

    # Select relavent features for analysis
    selected_vars = ['id', 'experiences_offered',
       'transit', 'host_since', 'host_response_rate',
       'host_acceptance_rate', 'host_is_superhost',
       'host_total_listings_count', 'host_verifications',
       'host_has_profile_pic', 'host_identity_verified', 'market', 
       'zipcode', 'is_location_exact', 'property_type', 'room_type',
       'accommodates', 'bathrooms', 'bedrooms', 'beds', 'bed_type',
       'amenities', 'square_feet', 'price', 'security_deposit', 'cleaning_fee',
       'guests_included', 'extra_people', 'minimum_nights',
       'maximum_nights', 'has_availability',
       'number_of_reviews',
       'review_scores_rating',
       'review_scores_accuracy', 'review_scores_cleanliness',
       'review_scores_checkin', 'review_scores_communication',
       'review_scores_location', 'review_scores_value',
       'requires_license', 
       'instant_bookable', 'cancellation_policy',
       'require_guest_profile_picture',
       'require_guest_phone_verification',
       'reviews_per_month']


**2.calender.csv**   
This Data set has log of property price per day. This data set can be used to determine how property prices are getting affected by the season.
The shape of the data is ` 1393570 records and 4 columns`. (High records since each listing id almost have 365 entries)
This dataset can be used to determine seasonal price swings in seattle airbnb. 

**3.reviews.csv**  
This data set has each review text details ever given for any property. The shape of the data is ` 84849 records and 6 columns`.


## Data Preparation:

### Data Pre-processing and 
From EDA, I could see that there are many features that needs to either removed or converted to correct data type for better analysis.
I consolidated and processed all such features in function ```preprocess_features(df):```
Following are the details of this function
    From looking at each of the categorical variable we found that,
    1. experiences_offered, host_verifications, market, has_availability, requires_license columns has only one value hence we need to Drop these columns from dataframe.
    2. 'security_deposit', 'cleaning_fee', 'extra_people' these columns need to be converted to float value of $
    3. amenities need to converted to int where number is total number of amenities
    4. host_response_rate need to converted to float
    5. Transit need to converted to nminal var where NaN = 0 and everuthing else is 1
    6. host_since need to converted to diff between 01/01/2016 - host_since in days

### Feature Exploration:
I derived few feateres from current data in order to answer the questions better

1. Facilities     : Sum of total number of faciliteis provided in property.
2. Host_eperience : Host experience in number of days
3. Transit        : Is Transit available from property, boolean. 
4. Lost days      : The number of days a property was available, i.e. it was not rented, is not benificial for the hosts since more the number of days property was available lesser the revenue from it. As a host, one would like to rent their property every single day of the year. I explored this target variable from Calender dataset of airbnb by summing up the number of avalilable days for a given property.  

   
### Imputation:
Function impute_features(df) imputes each feature depending on its type, categorical or continuous.
Below are its details
 
    This function cleans df using the following steps to produce X and y:
    1. For each numeric variable in X, fill the column with the mean value of the column.
    2. Lets drop all the rows which has nan values.
    3. Create dummy variables for categorical variables

### Splitting data:
I have used train_test_split function with test size 30% and train size 70% with random state =42.


## Modeling :
To understand the relationship between target and features I used supervised models. To be precise I used regression models and used coefficients of the model to decide which features are affecting target more/less.

### Regression:
For all the continuous targets I used multiple regression model with normalization.
### Logistic Regression:
For continuous target I used logistic regression model.


## Evaluation :
For evaluation I have used , R2 score.


## Results :

The analyzed data is from 2016, hence the predictions may not be relevant in current date; however it definitely gives us some intuition to understand the affecting factors in various target variables i analyzed here.

1. What are the primary factors affect the listing price?  
Ans : 
 
![](https://i.imgur.com/thrfVgC.png)  

Regreesion Model Score : 0.6850


2. What are the important features helps owner in becoming super host?
Ans:

![](https://i.imgur.com/FnYaqFB.png)

Logistic Regreesion Model Score : 0.7725


3. Which area in Seattle is most expensive to rent in 2016?

![](https://i.imgur.com/dTwAym0.png)

From above graph, it can be inferred that in 2016 area zipcode 98134 has highest mean listing price. 

4. Is there any seasonal effect in Seattle airbnb listing price? What would be best time to visit if I want to save some money?

![](https://i.imgur.com/MSHGdV0.png)

Looks like, January and February is the best month to travel it one want to save some moeny on airbnb rents.

5. Are there any homes in 2016 that never occupied? Is there any significant factors in why these places were never rented?

There are total : 678 listings, that were never rented in 2016

Below are the feature that are affecting lost days

![](https://i.imgur.com/zViJfCD.png)

Negative value on coefficient suggests negative correlation and vice varsa