# Introduction
A comprehensive analysis aimed at identifying patterns of customer disengagement, offering practical insights and recommendations to improve customer loyalty, decrease churn, and drive the company’s long-term growth.
# Tools I Used
For my in-depth exploration of the Telco customer churn dataset, I utilized a range of essential tools:
- Python: The backbone of my analysis, allowing me to manipulate the dataset and uncover key insights.
- Pandas: A powerful Python library, suitable for handling the customer churn dataset.
- Matplotlib: A Python library, suitable for creating and customizing visualizations.
- Seaborn: A Pythonn library, suitable for creating and customizing visualizations with more flexibility and ease.
- Google colab: The code editor used for managing the dataset, writing and executing codes.
- Git & GitHub: The platform for showcasing my project.
# Data Cleaning Steps
- The customerID column was dropped because it was not important for the analysis.
- tenure and gender columns were renamed to maintain a consistent letter case.
# Key Insights and Recommendations
## Churn Rate by Gender
Here is the code I used to find the churn rate by gender
```Python
df['Churn2'] = df['Churn'].map({'Yes' : 1, 'No' : 0})
churn_rate = int(round(df['Churn2'].sum()/len(df) * 100,0))
df_churn = df.groupby('Gender').agg(Count = ('Gender', 'count'), Churned = ('Churn2', 'sum')).reset_index()
df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
sns.countplot(data = df, x = 'Gender', hue = 'Churn', width = 0.5)
plt.title('Churn Rate by Gender')
plt.ylabel('Number of Customers')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
for i in df_churn.index:
  plt.text(i, df_churn['Churned'].iloc[i] + 20, f"{df_churn['Churn_rate'].iloc[i]:.1f}%")
plt.show()
```
![Image](https://github.com/user-attachments/assets/d32f14eb-eff3-4c09-83c6-57f7e797f096?raw=true)

A churn rate of 26.9% was observed among females, while a churn rate of  26.2% was observed among males. This implies that gender did not significantly influence churn.
## Churn Rate by Senior Citizen Status
Here is the code I used to find the churn rate by Senior Citizen Status
```Python
df['SeniorCitizenStatus'] = df['SeniorCitizen'].map({ 1: 'Yes', 0: 'No'})
df_churn = df.groupby('SeniorCitizenStatus').agg(Count = ('SeniorCitizenStatus', 'count'), Churned = ('Churn2', 'sum')).reset_index()
df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
sns.countplot(data = df, x = 'SeniorCitizenStatus', hue = 'Churn', width = 0.5)
plt.title('Churn Rate by Senior Citizen Status')
plt.ylabel('Number of Customers')
plt.xlabel('Senior Citizen Status')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
for i in df_churn.index:
  plt.text(i, df_churn['Churned'].iloc[i] + 20, f"{df_churn['Churn_rate'].iloc[i]:.1f}%")
plt.show()
```
![Image](https://github.com/user-attachments/assets/2819b4bc-bd6a-4ca0-89ba-3b46afdb5b10?raw=true)

A higher churn rate (41.7%) was observed in senior citizens, while about 23.6% of non-senior citizens churned. This shows that the potential of customers churning increase as they age. The company can lower churn rate by launching campaigns to target younger people, offering senior-friendly plans, or offering suitable incentives to retain older customers.
## Churn Rate by Living Situation
Here is the code I used to find the churn rate by living situation
```Python
living_situation = ['Partner', 'Dependents']
df_living_situation = []
for i, df_situation in enumerate(living_situation):
  df_churn = df.groupby(df_situation).agg(Count = (df_situation, 'count'), Churned = ('Churn2', 'sum')).reset_index()
  df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
  df_living_situation.append(df_churn)
fig, ax = plt.subplots(2,1, figsize = (7,7))
for i, situation in enumerate(living_situation):
  sns.countplot(data = df, x = living_situation[i], hue = 'Churn', width = 0.5, order = ['No', 'Yes'], ax = ax[i])
  ax[i].set_title(f'Churn Rate by {living_situation[i]}')
  ax[i].set_ylabel('Number of Customers')
  ax[i].set_xlabel(f'{living_situation[i]} Status')
  ax[i].yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
  for j in df_living_situation[i].index:
    ax[i].text(j, df_living_situation[i]['Churned'].iloc[j] + 20, f"{df_living_situation[i]['Churn_rate'].iloc[j]:.1f}%")
fig.suptitle('Churn Rate by Living Stuation')
plt.tight_layout()
plt.show()
```
![Image](https://github.com/user-attachments/assets/b714e095-7577-4656-9002-ec52ffc9d12d?raw=true)

A higher churn rate was observed among customers without partners and dependents (33.% and 31.3% respectively), while a lower churn rate was observed among those with partners and dependents (19.7% and 15.5% respectively).
## Churn Rate by Tenure
Here is the code I used to find the churn rate by tenure
```Python
bins = [0, 18, 36, 54, 72]
labels = ['Newcomers', 'Regulars', 'Veterans', 'Loyalists']
df['TenureDistribution'] = pd.cut(df['Tenure'], bins = bins, labels = labels, right = True)
df_churn = df.groupby('TenureDistribution', observed = True).agg(Count = ('TenureDistribution', 'count'), Churned = ('Churn2', 'sum')).reset_index()
df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
sns.countplot(data = df, x = 'TenureDistribution', hue = 'Churn', width = 0.5)
plt.title('Churn Rate by Tenure')
plt.ylabel('Number of Customers')
plt.xlabel('Tenure Distribution')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
for i in df_churn.index:
  plt.text(i, df_churn['Churned'].iloc[i] + 20, f"{df_churn['Churn_rate'].iloc[i]:.1f}%")
plt.show()
```
![Image](https://github.com/user-attachments/assets/76217933-2fb7-48e6-8043-545160df67fa?raw=true)

New comers (customers who used the service between 1 and 18 months) had the highest churn rate (44.6%), Regulars (customers who used the service between 19 and 36 months) had a churn rate of 22.7%, Veterans (customers who used the service between 37 and 54 months) 18% and Loyalists (customers who used the service between 55 and 72 months) had the lowest  churn rate of 8%. This implies that the possibility of churning decreases as the time of usage of the service increases and the company struggles with retaining new customers. This could be as a result of familiarity with the service. The company can reduce churn by making the service easier to use, offer solutions to challenges new users might be experiencing promptly, and offer flexible plans and discounts to encourage continued usage. Also, new users should be surveyed to identify and address dissatisfaction.
## Churn Rate by Type of Service
Here is the code I used to find the churn rate by type of service
```Python
service_type = df.columns[5:14].tolist()
df_services = []
for i, df_service in enumerate(service_type):
  df_churn = df.groupby(df_service).agg(Count = (df_service, 'count'), Churned = ('Churn2', 'sum')).reset_index()
  df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
  df_services.append(df_churn)
fig, ax = plt.subplots(9,1, figsize = (7,28))
for i, service in enumerate(service_type):
  sns.countplot(data = df, x = service_type[i], hue = 'Churn', width = 0.5, order = df_services[i].iloc[:,0].to_list(), ax = ax[i])
  ax[i].set_title(f'Churn Rate by {service_type[i]}')
  ax[i].set_ylabel('Number of Customers')
  ax[i].set_xlabel(f'{service_type[i]} Status')
  ax[i].yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
  for j in df_services[i].index:
    ax[i].text(j, df_services[i]['Churned'].iloc[j] + 20, f"{df_services[i]['Churn_rate'].iloc[j]:.1f}%")
fig.suptitle('Churn Rate by Type of Service')
plt.tight_layout()
plt.subplots_adjust(top=0.96)
plt.show()
```
### Churn rate by Phone Service
![Image](https://github.com/user-attachments/assets/138c1b9e-b808-4b04-b8ca-9a5162816ecd?raw=true)

A higher churn rate (26.7%) was observed among those that used this service, while those that didn't use it had a churn rate of 24.9%. This implies that churn rate is not significantly influenced by the use of Phone Service.
### Churn rate by Multiple Lines
![Image](https://github.com/user-attachments/assets/d8aad63d-b61f-4660-b19d-f4d15e022434?raw=true)

A higher churn rate (28.6%) was observed among those that used this service, while those that didn't use it had a churn rate of 25%. This implies that churn rate is not significantly influenced by the use of multiple lines.
### Churn rate by Internet Service
![Image](https://github.com/user-attachments/assets/54673668-e97d-4722-92db-edf4298a3c1a?raw=true)

The highest churn rate (41.9%) was observed among those that used fibre optic, followed by  those that used DSL (19%), and the least (7.4%) was observed among those that didn't use any internet service. The significantly higher churn rate among the services could be as a result of poor and inconsistent internet connections and other challenges. The customers using these services should be surveyed, be given prompt customer services whenever they are in need and the quality of service should be improved.
### Churn rate by Online Security
The highest churn rate (41.8%) was observed among those that did not use this service, followed by  those that used it  (14.6%), and the least (7.4%) was observed among those that didn't use any internet service. The significantly higher churn rate among those that didn't use the service could be as a result of security challenges. Churn rate can be reduced by improving the overall security of the service (even for those that do not subscribe to online security) or offering online security services at a discount to encourage users to adopt it.
### Churn rate by Online Backup
The highest churn rate (39.9%) was observed among those that did not use this service, followed by  those that used it  (21.5%), and the least (7.4%) was observed among those that didn't use any internet service. The Churn rate can be reduced by improving the quality of the service and organizing onboarding programs for users that might be interested in using it but not tech savvy enough.
### Churn rate by Device Protection
A higher churn rate (39.1%) was observed among those that did not use this service, while those that used it had a churn rate of 22.5%.
### Churn rate by  Tech Support
A higher churn rate (41.6%) was observed among those that did not use this service, while those that used it had a churn rate of 15.2%. This could be as a result of unresolved challenges due to lack of support. Churn rate can be reduced by offering discounts on tech support to ensure everyone have access to it.
### Churn Rate by StreamingTV
A higher churn rate (33.5%) was observed among those that did not use this service, while those that used it had a churn rate of 30.1%. This implies that this service does not significantly influence churn.
### Churn rate by Streaming Movies
A higher churn rate (33.7%) was observed among those that did not use this service, while those that used it had a churn rate of 29.9%. This implies that this service does not significantly influence churn.
## Churn Rate by Contract
Here is the code I used to find the churn rate by contract
```Python
df_churn = df.groupby('Contract').agg(Count = ('Contract', 'count'), Churned = ('Churn2', 'sum')).reset_index()
df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
sns.countplot(data = df, x = 'Contract', hue = 'Churn', width = 0.5)
plt.title('Churn Rate by Contract')
plt.ylabel('Number of Customers')
plt.xlabel('Contract Type')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
for i in df_churn.index:
  plt.text(i, df_churn['Churned'].iloc[i] + 20, f"{df_churn['Churn_rate'].iloc[i]:.1f}%")
plt.show()
```
The churn rate was observed to be highest among customers on a month-to-month contract (11.3%), followed by one year subscribers (46%) and least among two year subscribers (2.8%). Highest churn rate among monthly subscribers could be as a result of customer loyalty and familiarity developed from long term use of the service. Churn can be reduced by offering initial incentives and discounts on long term contracts and organizing onboarding programs to create familiarity.
## Churn Rate by Payment Method
Here is the code I used to find the churn rate by payment method
```Python
df_churn = df.groupby('PaymentMethod').agg(Count = ('PaymentMethod', 'count'), Churned = ('Churn2', 'sum')).reset_index()
df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
sns.countplot(data = df, x = 'PaymentMethod', hue = 'Churn', width = 0.5, order = df_churn.iloc[:, 0].to_list())
plt.title('Churn Rate by PaymentMethod')
plt.ylabel('Number of Customers')
plt.xlabel('Payment Method')
plt.xticks(rotation = 45, ha = 'right')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
for i in df_churn.index:
  plt.text(i, df_churn['Churned'].iloc[i] + 20, f"{df_churn['Churn_rate'].iloc[i]:.1f}%")
plt.show()
```
A churn rate of 45.3% was observed among customers that used electronic check, 19.1% among mailed check users, 16.7% among bank transfer users and 15.2% among credit card users. Electronic check users having the highest churn rate could be as a result of the complexity and friction associated with the use of the payment method. Churn can be reduced by streamlining the use of checks and encouraging the adoption of automated payment methods.
## Churn Rate by Charges
Here is the code I used to find the churn rate by charges
```Python
labels1 = ['Very Low', 'Low', 'Medium', 'High', 'Very High']
bins1 = [0, 24, 48, 72, 96, 120]
labels2 = ['Very Low', 'Low', 'Medium', 'High', 'Very High']
bins2 = [0, 1737, 3474, 5211, 6948, 8685]
df['Monthly Charges Distribution'] = pd.cut(df['MonthlyCharges'], bins = bins1, labels = labels1, right = True)
df['Total Charges Distribution'] = pd.cut(df['TotalCharges'], bins = bins2, labels = labels2, right = True)
Charges = ['Monthly Charges Distribution', 'Total Charges Distribution']
df_charges = []
for i, df_charge in enumerate(Charges):
  df_churn = df.groupby(df_charge, observed = True).agg(Count = (df_charge, 'count'), Churned = ('Churn2', 'sum')).reset_index()
  df_churn['Churn_rate'] = round(df_churn['Churned']/df_churn['Count'] * 100, 1)
  df_charges.append(df_churn)
fig, ax = plt.subplots(2,1, figsize = (7,7))
for i, charge in enumerate(Charges):
  sns.countplot(data = df, x = Charges[i], hue = 'Churn', width = 0.5, order = df_charges[i].iloc[:, 0].to_list(), ax = ax[i])
  ax[i].set_title(f'Churn Rate by {Charges[i]}')
  ax[i].set_ylabel('Number of Customers')
  ax[i].set_xlabel(f'{Charges[i]}')
  ax[i].yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f"{y/1000}K"))
  for j in df_charges[i].index:
    ax[i].text(j, df_charges[i]['Churned'].iloc[j] + 20, f"{df_charges[i]['Churn_rate'].iloc[j]:.1f}%")
fig.suptitle('Churn Rate by Charges')
plt.tight_layout()
plt.show()
```
### Churn rate by Monthly Charges
A churn rate of 37.2% was observed among customers with high monthly charges, 31.4% among very high monthly charges, 23.7% among medium monthly charges, 23% among low monthly charges and 8.7% among very low monthly charges. Churn rate can be reduced by revising the prices to ensure users get good value for their money.
### Churn rate by Total Charges
A churn rate of 32.9% was observed among customers with very low total charges, 24.6% among low total charges, 16.7% among medium total charges, 14.7% among high total charges and 10.7% among very high charges. This trend can be as a result of user engagement and long term use of the service. Churn can be reduced by getting customers to use the service for long through campaigns, incentives and discounts on long term contracts.
# Conclusion
The project showcased and strengthened my Python skills while offering valuable insights into customer churn, which can aid in enhancing customer retention and supporting the company’s overall growth.
