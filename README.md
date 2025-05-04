This project analyzes a dataset containing information about various marketing channels, their associated costs, and 
customer acquisition performance. Through calculation of key performance metrics and visual representations, it helps
identify which marketing channels deliver the best value and efficiency.

Data Source
The analysis uses data from "customer_acquisition_cost_dataset.csv" which appears to contain:

Marketing channel names
Marketing spend amounts
Number of new customers acquired per channel


Key Metrics Calculated

Customer Acquisition Cost (CAC): The cost to acquire a single customer through each marketing channel

Formula: Marketing Spend ÷ New Customers


Conversion Rate: The percentage efficiency of turning marketing spend into customers

Formula: (New Customers ÷ Marketing Spend) × 100


Break-Even Customers: The number of customers needed to justify the marketing expenditure

Formula: Marketing Spend ÷ CAC



Visualizations & Insights
The project creates several interactive visualizations using Plotly:

CAC by Marketing Channel (Bar Chart):

Directly compares the cost efficiency of each channel
Helps identify which channels have the lowest customer acquisition costs


New Customers vs. CAC (Scatter Plot with Trendline):

Shows the relationship between volume of customers and cost
The trendline helps identify if there are economies or diseconomies of scale
Color-coding by marketing channel allows for multi-dimensional analysis


Conversion Rates by Marketing Channel (Bar Chart):

Provides insight into which channels are most efficient at converting spend to customers
Higher conversion rates indicate more efficient channels


Break-Even Customers by Marketing Channel (Bar Chart):

Shows the theoretical minimum number of customers needed to justify each channel's spend


Actual vs. Break-Even Customers Comparison (Grouped Bar Chart):

Direct visual comparison of performance against break-even thresholds
Highlights channels performing above or below expectations
Helps identify which channels should receive more or less investment



Business Applications
This analysis project provides marketing teams and business stakeholders with:

Clear identification of the most cost-effective marketing channels
Evidence-based insights for reallocating marketing budget
Performance benchmarks for evaluating marketing strategies
Visual tools for communicating ROI to management and stakeholders

The project demonstrates a data-driven approach to marketing decision-making, replacing gut feeling 
with quantitative analysis and visualization to maximize return on marketing investments.






#importing the necessary Python libraries and the dataset

import pandas as pd
import plotly.express as px
import plotly.io as pio
import plotly.graph_objects as go
pio.templates.default = "plotly_white"



data = pd.read_csv("customer_acquisition_cost_dataset.csv")
print(data.head())

Customer_ID Marketing_Channel  Marketing_Spend  New_Customers
0    CUST0001   Email Marketing      3489.027844             16
1    CUST0002        Online Ads      1107.865808             33
2    CUST0003      Social Media      2576.081025             44
3    CUST0004        Online Ads      3257.567932             32
4    CUST0005   Email Marketing      1108.408185             13






#Let’s have a look at the column insights 
data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 500 entries, 0 to 499
Data columns (total 4 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   Customer_ID        500 non-null    object 
 1   Marketing_Channel  500 non-null    object 
 2   Marketing_Spend    500 non-null    float64
 3   New_Customers      500 non-null    int64  
dtypes: float64(1), int64(1), object(2)
memory usage: 15.8+ KB



#calculate the customer acquisition cost:

data['CAC'] = data['Marketing_Spend'] / data['New_Customers']


#Let's examine how customer acquisition costs vary across our different marketing channels

fig1 = px.bar(data, x='Marketing_Channel', 
              y='CAC', title='CAC by Marketing Channel')
fig1.show()

![CAC-1](https://github.com/user-attachments/assets/6e141773-92f6-4aee-a700-8853fc7067a4)

Email marketing shows the most expensive customer acquisition rate, while social media 
demonstrates the most cost-effective approach. Next, let's explore how the number of 
new customers correlates with acquisition costs across channels


fig2 = px.scatter(data, x='New_Customers', 
                  y='CAC', color='Marketing_Channel', 
                  title='New Customers vs. CAC', 
                  trendline='ols')
fig2.show()

![CAC-2](https://github.com/user-attachments/assets/c996e8b5-540c-4fad-9d1b-9a6f99ae9856)

The downward-trending line in this visualization indicates that marketing channels attracting
more customers typically achieve lower acquisition costs per customer, suggesting economies of 
scale in our customer acquisition efforts



#Now let’s have a look at the summary statistics of all the marketing channels

summary_stats = data.groupby('Marketing_Channel')['CAC'].describe()
print(summary_stats)

                   count        mean        std        min        25%  \
Marketing_Channel                                                       
Email Marketing    124.0  132.913758  89.597107  23.491784  68.226195   
Online Ads         130.0  122.135938  79.543793  24.784414  62.207753   
Referral           128.0  119.892174  74.101916  22.012364  71.347939   
Social Media       118.0  126.181913  77.498788  21.616453  75.633389   

                          50%         75%         max  
Marketing_Channel                                      
Email Marketing    106.940622  177.441898  434.383446  
Online Ads          97.736027  163.469540  386.751285  
Referral            99.835688  137.577935  366.525209  
Social Media       102.620356  167.354709  435.487346  


Analyzing these statistical insights enables you to.

--Use the mean CAC values to compare the average cost of customer acquisition across different
Marketing Channels. For example, if minimizing CAC is a priority, you may want to focus on 
channels with lower average CAC values.

--Use the standard deviation to assess the consistency of CAC within each channel. Higher
standard deviations suggest greater variability, which may require further investigation to 
understand the reasons behind the fluctuation in costs.

--Use quartiles to understand a sense of the distribution of CAC values. For example, if you want to 
target cost-effective customer acquisition, you might focus on channels where the first quartile
(25%) has relatively low CAC values.

--Similarly, the minimum and maximum CAC values give you an idea of the range of costs associated 
with each channel, helping you understand the potential cost


 
 
 #Calculate the conversion rate of the marketing campaign

 data['Conversion_Rate'] = data['New_Customers'] / data['Marketing_Spend'] * 100


 Here are the insights into the conversion rate by marketing channel:

# Conversion Rates by Marketing Channel
fig = px.bar(data, x='Marketing_Channel', 
             y='Conversion_Rate', 
             title='Conversion Rates by Marketing Channel')
fig.show()


 ![CAC-3](https://github.com/user-attachments/assets/709d47c3-bb21-4529-983d-f1bb8e77b2ce)

We can observed that online ads have the highest conversion rates compared to all other marketing channels



#let’s calculate the break-even customers for this marketing campaign.

data['Break_Even_Customers'] = data['Marketing_Spend'] / data['CAC']

fig = px.bar(data, x='Marketing_Channel', 
             y='Break_Even_Customers', 
             title='Break-Even Customers by Marketing Channel')
fig.show()


![CAC-4](https://github.com/user-attachments/assets/ac4e1258-c666-42a7-bfad-bd5d7b63b59c)



#let’s compare the actual customers acquired with the break-even customers for each marketing channel

fig = go.Figure()




# Actual Customers Acquired
fig.add_trace(go.Bar(x=data['Marketing_Channel'], y=data['New_Customers'],
                     name='Actual Customers Acquired', marker_color='royalblue'))

# Break-Even Customers
fig.add_trace(go.Bar(x=data['Marketing_Channel'], y=data['Break_Even_Customers'],
                     name='Break-Even Customers', marker_color='lightcoral'))

# Update the layout
fig.update_layout(barmode='group', title='Actual vs. Break-Even Customers by Marketing Channel',
                  xaxis_title='Marketing Channel', yaxis_title='Number of Customers')

# Show the chart
fig.show()


![CAC-5](https://github.com/user-attachments/assets/684bb4de-4a16-4c3b-92bc-6ed69e61606d)


This indicates a successful outcome of the marketing campaign, as the number of actual customers
acquired from each channel precisely meets the break-even point. Had the acquired customers fallen
short of this threshold, it would have signaled the need to reevaluate marketing strategies or 
consider reallocating resources to improve performance.


