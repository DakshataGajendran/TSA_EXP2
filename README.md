# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


# Ensure 'AirPassengers.csv' is in your working directory
data = pd.read_csv('AP.csv', parse_dates=['Month'], index_col='Month')

resampled_data = data['#Passengers'].resample('Y').sum().to_frame()
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Month': 'Year'}, inplace=True)

years = resampled_data['Year'].tolist()
passengers = resampled_data['#Passengers'].tolist()
n = len(years)

```
### A - LINEAR TREND ESTIMATION

```
X = [i - years[n // 2] for i in years]
x2 = [i**2 for i in X]
xy = [i * j for i, j in zip(X, passengers)]

b = (n * sum(xy) - sum(passengers) * sum(X)) / (n * sum(x2) - (sum(X)**2))
a = (sum(passengers) - b * sum(X)) / n

linear_trend = [a + b * x_val for x_val in X]
resampled_data['Linear Trend'] = linear_trend

```
## B- POLYNOMIAL TREND ESTIMATION
```
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i * j for i, j in zip(x2, passengers)]

coeff_matrix = [[n, sum(X), sum(x2)],
                [sum(X), sum(x2), sum(x3)],
                [sum(x2), sum(x3), sum(x4)]]

Y_vector = [sum(passengers), sum(xy), sum(x2y)]

# Solving for a, b, c in y = a + bx + cx^2
solution = np.linalg.solve(coeff_matrix, Y_vector)
a_poly, b_poly, c_poly = solution

poly_trend = [a_poly + b_poly * x_val + c_poly * (x_val**2) for x_val in X]
resampled_data['Polynomial Trend'] = poly_trend
```
```
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x^2")

# --- Visualization: Separate Graphs ---

# Plot 1: Linear Trend Estimation
plt.figure(figsize=(10, 5))
plt.plot(resampled_data['Year'], resampled_data['#Passengers'], color='blue', marker='o', label='Actual')
plt.plot(resampled_data['Year'], resampled_data['Linear Trend'], color='black', linestyle='--', label='Linear Trend')
plt.title('Linear Trend Estimation')
plt.xlabel('Year')
plt.ylabel('Passengers')
plt.legend()
plt.grid(True)
plt.show()

# Plot 2: Polynomial Trend Estimation
plt.figure(figsize=(10, 5))
plt.plot(resampled_data['Year'], resampled_data['#Passengers'], color='blue', marker='o', label='Actual')
plt.plot(resampled_data['Year'], resampled_data['Polynomial Trend'], color='black',linestyle='--',label='Polynomial Trend')
plt.title('Polynomial Trend Estimation (Degree 2)')
plt.xlabel('Year')
plt.ylabel('Passengers')
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT
## A - LINEAR TREND ESTIMATION
<img width="1194" height="602" alt="image" src="https://github.com/user-attachments/assets/1155cd12-1276-4fff-961a-256a1b962be1" />


## B- POLYNOMIAL TREND ESTIMATION
<img width="1146" height="605" alt="image" src="https://github.com/user-attachments/assets/cbd90504-8c20-4a06-ad1d-71257cde30c8" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
