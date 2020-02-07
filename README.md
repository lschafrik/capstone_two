<p align="center"><img src="pictures/Rent_the_Runway_Logo%20copy.png" width="200" /></p>

# Overview
***

### Background
- Rent the Runway was officially founded in 2009, and now boasts over 9 million members.  According to their site, Rent the Runway was founded in order to bring empowerment and self-confidence to women around the world by providing an unlimited closet with which users can freely express themselves and dress for success.  Rent the Runway's shared closet means greater sustainability - 89% of users buy fewer clothes than they used to, and garments are used to the extent of their lifecycle, after which they may be donated to various organizations.  The company also launched their 'Rent the Runway Foundation' in 2015, whose mission is to encourage women in entrepreneurship.

### Data
- I found the initial dataset on Kaggle, which can be viewed [here](https://www.kaggle.com/rmisra/clothing-fit-dataset-for-size-recommendation).  The dataset includes __192,544__ total entries and columns such as *rating, review_text, and review_summary*
- Each row corresponds with a user's review of their garment rental, with data gathered from 2011 to 2018, and includes their basic profile information.
- Previously (Capstone One), NaN values were removed, and columns such as weight, height, and date were transformed into numerical values.  Moving forward, this is the data used.

![Screen%20Shot%202020-02-07%20at%2012.08.58%20PM.png](pictures/Screen%20Shot%202020-02-07%20at%2012.08.58%20PM.png)

# Creating a Model
***
<u>The Question:</u> __*Can I predict a user's rating based on review_summary and review_text?*__

### Preparing the Data
- The rating options are as follows: 2, 4, 6, 8, or 10, with 2 as the lowest score possible.


- I initially attempted to fit my entire dataset (after putting it through train_test_split) to a model.  This became an issue, as my computer couldn't handle this and crashed multiple times.  I also realized that my data was wildly imbalanced.
    - *I resolved this issue by creating a function to __randomly sample a specified amount of data per rating__.  Not only did this save my poor computer, it also improved my model's score - neat!  After some inquiry, I found that randomly sampling __700 entries per rating__ best improved my model.*
    
![Screen%20Shot%202020-02-07%20at%2012.24.49%20PM.png](pictures/Screen%20Shot%202020-02-07%20at%2012.24.49%20PM.png)

- I found, after running my sampled data through an NLP pipeline, that a number of results were empty, which I corrected in two parts.
    - *First, I noticed many users entered only one or two words as their review, or treated the review_summary as the review_text, entering their entire review into the summary box and only one or two words into the text box.  Because of this, I __combined the two columns__ and treated it as one full review.*

<p align="center"><img src="pictures/Screen%20Shot%202020-02-07%20at%201.10.19%20PM.png" /></p>

- This definitely helped, but there was still another issue to face.
    - *Second, I found that even with this fix, many reviews were empty or contained a very small amount of words, which negatively affected my model.  I corrected this by creating a function to __remove a row if the word count fell below a specified minimum count.__  I found specifying a __minimum of 8 words__ gave me the best results.*
        - Interestingly, the majority of rows dropped had the lowest rating.
    
![Screen%20Shot%202020-02-07%20at%2012.35.30%20PM.png](pictures/Screen%20Shot%202020-02-07%20at%2012.35.30%20PM.png)

### Fitting the Data to a Model
- I created an NLP pipeline, running the text-summary data through CountVectorizer and TfidfTransformer, taking a moment to study a comparison of key words between the lowest and highest ratings.

![Screen%20Shot%202020-02-07%20at%2012.52.47%20PM.png](pictures/Screen%20Shot%202020-02-07%20at%2012.52.47%20PM.png)

### Tweaking the Model
- My first attempts were using a Classifier, but as my ratings were numeric I switched to Regressor models
    - *I compared a Gradient Boosting Regressor model with a Random Forest Regressor and found the Gradient Boosting model gave me the best results from my training and testing data with the following parameters.*

<p align="center"><img src="pictures/Screen%20Shot%202020-02-07%20at%2012.56.38%20PM.png" /></p>

- Since the model predictions were continuous numeric results, I considered any prediction within a 0.5 range on either side of the actual rating as ‘accurate’, and graphed these results.
    - *Again, we see here that the lowest rating shows interesting results.  The model also has trouble predicting a rating of 10 with the testing data.  One thing we do have to keep in mind here is that there are no predictions lower than 2 or higher than 10, so that could certainly be affecting these results.*

![Screen%20Shot%202020-02-07%20at%201.00.15%20PM.png](pictures/Screen%20Shot%202020-02-07%20at%201.00.15%20PM.png)

# Conclusion
***

### *After this capstone, what suggestions would you provide for improvements?*
- I would recommend __limiting the character count for the review_summary box__, as some users would put one or two words here and their full review in the review_text box, and others would do exactly the opposite.


### *What future improvements or inquiries would you pursue?*
- I would see how well I could predict other features using the text-summary column
- I would determine the impact the other columns have on a user’s rating
- I would continue working with my current model to find a better score
