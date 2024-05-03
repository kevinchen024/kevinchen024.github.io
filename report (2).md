---
layout: wide_default
---    
# 1. Summary Section

The primary goal of my repositories and report is to automate the extraction of filing dates from 10-K documents and correlate these dates with stock returns for analysis. My analysis includes parsing file paths to retrieve unique identifiers (like CIK and Accession Numbers), fetching document details from the SEC's EDGAR database, and conducting sentiment analysis on the content of 10-K reports to gauge positive and negative discussions surrounding key topics like sales, places, and risks. This also involves processing text from 10-K reports and leveraging regex for data extraction.

I learned a lot thorough this task: 
1. Effective Extraction and Parsing: The methodology for extracting identifiers from file paths and fetching filing dates from EDGAR proved effective, contingent on accurate regex patterns and the consistent structure of EDGAR URLs.

2. Challenges I faced in Sentiment Analysis: The attempt to classify mentions of specific topics as positive or negative highlighted the complexities of sentiment analysis. While keyword-based approaches provided initial insights, they also underscored the limitations of such methods in accurately capturing sentiment without considering linguistic nuances and context.

3. Importance of Data Organization: Organizing the extracted information into DataFrames emphasized the utility of pandas for handling and analyzing structured data. However, it also revealed the necessity of meticulous variable management and execution order in scripting.


# 2. Data Section

#1. The sample consists of publicly traded companies listed in the S&P 500 index for the year 2022. Each company in the sample has filed a 10-K report with the SEC for the fiscal year ending in 2022. 

#2. My code: 

"url = 'https://github.com/LeDataSciFi/data/raw/main/Stock%20Returns%20(CRSP)/crsp_2022_only.zip'
response = requests.get(url)
with ZipFile(BytesIO(response.content)) as z:
    file_name = z.namelist()[0]
    with z.open(file_name) as f:
        crsp_df = pd.read_stata(f)

crsp_df['date'] = pd.to_datetime(crsp_df['date'])
crsp_df = crsp_df.sort_values('date')  # Explicitly sort by date


return_df['Filing Date'] = pd.to_datetime(return_df['Filing Date'], errors='coerce')


return_df['Return on Filing Day'] = None

for index, row in return_df.dropna(subset=['Filing Date']).iterrows():
    filing_date = row['Filing Date']
    # Proceed with filtering crsp_df as before
    nearest_after = crsp_df[crsp_df['date'] >= filing_date].head(1)
    
return_df

#3. a. "positive_words_pattern = "|".join(BHR_positive)"- I did a predefined list of words that are generally indicative of positive sentiment.

  # b. "Topic Words List (wordsl1)"- I did a list of keywords related to specific topics of interest, like 'sales', 'competition', 'regulation'. 
   
  # c. Then I combined words into patterns, which is the lists of positive words and topic words are each joined into a single string pattern. 
   
  # d. Then I used egex Pattern (sales_positive_regex)
   
  # e. Lastly to find the sentiment score, which is calculated as the ratio of the number of positive mentions (hits) to the total document length in words (doc_length). This score represents the proportion of the document that discusses the specified topics in a positive light.
   
   #To sum up, I think this approach combines lexical resources (lists of positive and topic-specific words) with pattern matching to extract meaningful sentiment indicators from unstructured text data.
   

    #a. ML negative dictionary: 

    "negative_words = file.read().splitlines()  
    num_negative_words = len(negative_words)  
    print(f"The ML negative dictionary contains {num_negative_words} words.")"
    
    #The ML negative dictionary contains 94 words.
    
    #b. ML positive dictionary:
    
    "BHR_positive = file.read().splitlines()  
     num_positive_words = len(BHR_positive)
     print(f"The ML positive dictionary contains {num_positive_words} words.")"

    #The ML positive dictionary contains 75 words.
    
    #c. LM negative dictionary:
    
    "negative_words_df = df[df['Negative'] > 0]
    LM_negative = negative_words_df['Word'].str.lower().tolist()
    num_LM_negative_words = len(LM_negative)"
    print(f"The LM negative dictionary contains {num_LM_negative_words} words.")
    
    #The LM negative dictionary contains 2345 words.
    
    #d. LM positive dictionary:
    
    positive_words_df = df[df['Positive'] > 0]
    LM_positive = positive_words_df['Word'].str.lower().tolist()
    num_LM_positive_words = len(LM_positive)
    print(f"The LM positive dictionary contains {num_LM_positive_words} words.")

    #The LM positive dictionary contains 347 words.
    
#5. a. Sales, Competition, Regulation

    #Sales: Directly correlates with a company's revenue-generating capability.
    
    #Competition: Provides insights into a company's market position and competitive environment.
    
    #Regulation: Especially important in industries that are heavily regulated. 
    
    #b. Fraud, Disaster, Compensation
    
    #Fraud: Mention of fraud, especially in a negative context, is a red flag for investors. 

    #Disaster: In the context of risk management, discussions around disaster preparedness or recovery can be positive if they demonstrate resilience and strategic planning.

    #Compensation: Can indicate how well a company rewards and retains talent, which is crucial for long-term success.
    


# 3. Result

1. ![image.png](attachment:3f05d57c-21de-45ce-90cd-4bb9af92d5bc.png)


2. ![image.png](attachment:ae1c3195-e896-40d2-b996-b0fafa91f86b.png)

Four Discussion Topics:

    1. 

a. Correlation Analysis: Determine Pearson’s correlations between the various sentiment variables – LM Positive, ML Negative, ML Positive and LM Negative and the returns variable. Correlation coefficients can range from -1 to 1; a coefficient of -1 represents a perfect negative linear relationship while a coefficient of 1 is a perfect positive linear relationship and 0 reflects no linear relationship.

b. Regression Analysis: Perform linear regression on the returns variable as dependent variable including sentiments variables as independent variables. This can be done for each sentiment variable independently or include both positive and negative sentiments in the same model to control for their inter-effects

c. Visual Analysis: Plot scatter/binscatter plots for each sentiment variable against the returns variable to assess their relationships visually.

    2.

a. Sample Size and Composition: A larger sample of firms and years can lead to different results due to increased statistical power and the ability to capture more variability and nuances in sentiment and its impact on stock returns. Smaller samples might not fully represent the market's diversity.

b. Methodology in Sentiment Analysis: Different methodologies in extracting and quantifying sentiment can yield different outcomes. The Garcia, Hu, and Rohrer study might use more sophisticated or different sentiment analysis techniques, capturing sentiment nuances more effectively. 

c. Generalizability: A broader sample makes the findings more generalizable across different firms, industries, and market conditions. This helps in establishing the robustness of the relationship between sentiment and stock returns.

d. Temporal Robustness: Including more years in the study allows researchers to test whether the observed relationships hold across different market cycles, including bull and bear markets.

    3.

a. Sales Sentiment
Relationship with Returns: If the Sales Sentiment measure shows a statistically significant positive correlation with stock returns, this indicates that positive sentiment regarding a company's sales performance could be linked with higher returns.

b. Competition Sentiment
Relationship with Returns: If Competition Sentiment is significantly correlated with stock returns, it suggests that how a company discusses its competitive landscape can impact investor perceptions and, consequently, stock prices.

c. Regulatory Sentiment
Relationship with Returns: A significant correlation between Regulatory Sentiment and stock returns would imply that investor returns are sensitive to how companies perceive and communicate regulatory challenges or advantages.

    4. 

a. Differences in Sign:

Sales Sentiment typically would have a positive sign when correlated with stock returns. Positive sales sentiment reflects optimism about a company's revenue-generating capability, directly influencing investor perceptions and potentially leading to an increase in stock prices.

Competition Sentiment could have a more nuanced relationship with stock returns. A positive sign might emerge if the sentiment reflects a company's strong competitive position or successful strategies that can ward off competition. Conversely, a negative sign could result from sentiment expressing concerns about increasing competitive pressures that may threaten market share and profitability.

Regulatory Sentiment also presents a complex relationship. Positive regulatory sentiment (e.g., beneficial regulatory changes or successful compliance strategies) could correlate positively with stock returns. However, negative sentiment reflecting regulatory challenges or risks might lead to a negative correlation, as investors assess the potential for increased costs, fines, or operational restrictions.

b. Differences in Magnitude
The magnitude of these relationships can vary based on several factors:

Industry and Market Sensitivity: Industries highly sensitive to regulatory changes (e.g., healthcare, financial services) might show a stronger magnitude of correlation between regulatory sentiment and stock returns. Similarly, industries with intense competition could exhibit a stronger relationship between competition sentiment and returns.

Investor Perception and Time Horizons: The magnitude can also reflect investor perceptions and time horizons. Sales sentiment might have a more immediate impact on stock prices due to direct ties to revenue. In contrast, competition and regulatory sentiments may affect long-term strategic positioning, leading to a more gradual influence on stock returns.

Economic Conditions and Market Cycles: The broader economic environment and market cycles can amplify or mute the impact of sentiment on stock returns. For example, during economic downturns, positive sales sentiment might have a stronger positive impact due to its counter-cyclical nature.


```python

```
