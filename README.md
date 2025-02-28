# Developing a Neural Network Classification Model

### AIM
To develop a neural network classification model for the given dataset.

### Problem Statement
An automobile company has plans to enter new markets with their existing products. After intensive market research, they’ve decided that the behavior of the new market is similar to their existing market.
In their existing market, the sales team has classified all customers into 4 segments (A, B, C, D ). Then, they performed segmented outreach and communication for a different segment of customers. This strategy has work exceptionally well for them. They plan to use the same strategy for the new markets.
You are required to help the manager to predict the right group of the new customers.
## Neural Network Model
![2nd](https://github.com/nithin-popuri7/nn-classification/assets/94154780/93342d38-b206-47b2-9dad-e2384c4facb1)

## DESIGN STEPS

### STEP 1: 
Import the required packages

### STEP 2: 
Import the dataset to manipulate on

### STEP 3: 
Clean the dataset and split to training and testing data

### STEP 4: 
Create the Model and pass appropriate layer values according the input and output data

### STEP 5: 
Compile and fit the model

### STEP 6: 
Load the dataset into the model

### STEP 7: 
Test the model by predicting and output



## PROGRAM
```
Developed by:Arani Chopra Sai
Reg No:212220040011
```
### Importing the require packages
```
import pandas as pd
from sklearn.model_selection import train_test_split
from tensorflow.keras.models import Sequential
from tensorflow.keras.models import load_model
import pickle
from tensorflow.keras.layers import Dense
from tensorflow.keras.layers import Dropout
from tensorflow.keras.layers import BatchNormalization
import tensorflow as tf
import seaborn as sns
from tensorflow.keras.callbacks import EarlyStopping
from sklearn.preprocessing import MinMaxScaler
from sklearn.preprocessing import LabelEncoder
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import OrdinalEncoder
from sklearn.metrics import classification_report,confusion_matrix
import numpy as np
import matplotlib.pylab as plt
import sklearn.metrics as metrics
```
### Importing the dataset
```
customer_df = pd.read_csv('customers.csv')
```
### Data exploration
```
customer_df.columns
customer_df.dtypes
customer_df.shape
customer_df_cleaned.isnull().sum()
customer_df_cleaned.shape
customer_df_cleaned.dtypes
customer_df_cleaned['Gender'].unique()
customer_df_cleaned['Ever_Married'].unique()
customer_df_cleaned['Graduated'].unique()
customer_df_cleaned['Profession'].unique()
customer_df_cleaned['Spending_Score'].unique()
customer_df_cleaned['Var_1'].unique()
customer_df_cleaned['Segmentation'].unique()
## Encoding of input values
gories_list=[['Male', 'Female'],
           ['No', 'Yes'],
           ['No', 'Yes'],
           ['Healthcare', 'Engineer', 'Lawyer', 'Artist', 'Doctor',
            'Homemaker', 'Entertainment', 'Marketing', 'Executive'],
           ['Low', 'Average', 'High']
           ]
enc = OrdinalEncoder(categories=categories_list)
customers_1 = customer_df_cleaned.copy()

customers_1[['Gender',
             'Ever_Married',
              'Graduated','Profession',
              'Spending_Score']] = enc.fit_transform(customers_1[['Gender',
                                                                 'Ever_Married',
                                                                 'Graduated','Profession',
                                                                 'Spending_Score']])
```
### Encoding of output values
```
le = LabelEncoder()
customers_1['Segmentation'] = le.fit_transform(customers_1['Segmentation'])
customers_1 = customers_1.drop('ID',axis=1)
customers_1 = customers_1.drop('Var_1',axis=1)
customers_1['Segmentation'].unique()
X=customers_1[['Gender','Ever_Married','Age','Graduated','Profession','Work_Experience','Spending_Score','Family_Size']].values
y1 = customers_1[['Segmentation']].values
one_hot_enc = OneHotEncoder()
one_hot_enc.fit(y1)
y = one_hot_enc.transform(y1).toarray()
```
### Spliting the data
```
X_train,X_test,y_train,y_test=train_test_split(X,y,
                                               test_size=0.33,
                                               random_state=50)
X_train.shape
```
### Scaling the features of input
```
scaler_age = MinMaxScaler()
scaler_age.fit(X_train[:,2].reshape(-1,1))
X_train_scaled = np.copy(X_train)
X_test_scaled = np.copy(X_test)
X_train_scaled[:,2] = scaler_age.transform(X_train[:,2].reshape(-1,1)).reshape(-1)
X_test_scaled[:,2] = scaler_age.transform(X_test[:,2].reshape(-1,1)).reshape(-1)
Creation of model

ai_brain = Sequential([
  Dense(units = 8, input_shape=[8]),
  Dense(units =16, activation='relu'),
  Dense(units =4, activation ='softmax')
])
ai_brain.compile(optimizer='adam',
                 loss='categorical_crossentropy',
                 metrics=['accuracy'])
ai_brain.fit(x=X_train_scaled,y=y_train,
             epochs=2000,batch_size=256,
             validation_data=(X_test_scaled,y_test),
             )
```
### Ploting the metrics
```
metrics = pd.DataFrame(ai_brain.history.history)
metrics.head()
metrics[['accuracy','val_accuracy']].plot()
metrics[['loss','val_loss']].plot()
```
### Making the prediction
```
x_test_predictions = np.argmax(ai_brain.predict(X_test_scaled), axis=1)
x_test_predictions.shape
y_test_truevalue = np.argmax(y_test,axis=1)
y_test_truevalue.shape
print(confusion_matrix(y_test_truevalue,x_test_predictions))
print(classification_report(y_test_truevalue,x_test_predictions)
```
### Saving and loading the model
```
ai_brain.save('customer_classification_model.h5')
with open('customer_data.pickle', 'wb') as fh:
   pickle.dump([X_train_scaled,y_train,X_test_scaled,y_test,customers_1,customer_df_cleaned,scaler_age,enc,one_hot_enc,le], fh)
ai_brain = load_model('customer_classification_model.h5')
with open('customer_data.pickle', 'rb') as fh:
   [X_train_scaled,y_train,X_test_scaled,y_test,customers_1,customer_df_cleaned,scaler_age,enc,one_hot_enc,le]=pickle.load(fh)
```
### Making the prediction for single input
```
x_single_prediction = np.argmax(ai_brain.predict(X_test_scaled[1:2,:]), axis=1)
print(x_single_prediction)
print(le.inverse_transform(x_single_prediction))
```
### Dataset Information

<img width="838" alt="Dataset" src="https://github.com/saieswar1607/nn-classification/assets/93427011/ce70f2eb-08be-4067-8885-75697fb6176b">

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

<img width="643" alt="1" src="https://github.com/saieswar1607/nn-classification/assets/93427011/a7150d82-4f6b-4791-b7a8-07e1ea64298e">

### Classification Report

<img width="644" alt="2" src="https://github.com/saieswar1607/nn-classification/assets/93427011/a0f351d8-bd46-4b80-bf35-025610d244b2">

### Confusion Matrix

<img width="589" alt="3" src="https://github.com/saieswar1607/nn-classification/assets/93427011/75f87e56-eb87-4156-ba14-01fd0315d92b">

### New Sample Data Prediction

<img width="779" alt="4" src="https://github.com/saieswar1607/nn-classification/assets/93427011/8ac1fbea-7ac0-4637-84cb-89b617865ce3">

## RESULT
Therefore a Neural network classification model is developed and executed successfully.
