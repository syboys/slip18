# slip18


Q.1 Write a JAVA Program to implement built-in support (java.util.Observable) Weather
station with members temperature, humidity, pressure and methods
mesurmentsChanged(), setMesurment(), getTemperature(), getHumidity(),
getPressure() 
Q.2. Write a python program to implement Polynomial Linear Regression for given dataset

Q.3 Create your Django app in which after running the server, you should see on the browser,
the text “Hello! I am learning Django”, which you defined in the index view.


Q.1 Write a JAVA Program to implement built-in support (java.util.Observable) Weather
station with members temperature, humidity, pressure and methods
mesurmentsChanged(), setMesurment(), getTemperature(), getHumidity(),
getPressure()
:
import java.util.Observable;
import java.util.Observer;
class CurrentConditionsDisplay implements Observer, DisplayElement {
 Observable observable;
 private float temperature;
 private float humidity; 
public CurrentConditionsDisplay(Observable observable) {
 this.observable = observable;
 observable.addObserver(this);
 }
 public void update(Observable obs, Object arg) {
 if (obs instanceof WeatherData) {
 WeatherData weatherData = (WeatherData)obs;
 this.temperature = weatherData.getTemperature();
 this.humidity = weatherData.getHumidity();
 display();
 }
 }
 public void display() {
 System.out.println("Current conditions: " + temperature
 + "F degrees and " + humidity + "% humidity");
 }
}
interface DisplayElement {
 public void display();
}
class ForecastDisplay implements Observer, DisplayElement {
 private float currentPressure = 29.92f;
 private float lastPressure;
 public ForecastDisplay(Observable observable) {
 observable.addObserver(this);
 } 
public void update(Observable observable, Object arg) {
 if (observable instanceof WeatherData) {
 WeatherData weatherData = (WeatherData)observable;
 lastPressure = currentPressure;
 currentPressure = weatherData.getPressure();
 display();
 }
 }
 public void display() {
 System.out.print("Forecast: ");
 if (currentPressure > lastPressure) {
 System.out.println("Improving weather on the way!");
 } else if (currentPressure == lastPressure) {
 System.out.println("More of the same");
 } else if (currentPressure < lastPressure) {
 System.out.println("Watch out for cooler, rainy weather");
 }
 }
}
 class HeatIndexDisplay implements Observer, DisplayElement {
 float heatIndex = 0.0f;

 public HeatIndexDisplay(Observable observable) {
 observable.addObserver(this);
 }
public void update(Observable observable, Object arg) {
 if (observable instanceof WeatherData) {
 WeatherData weatherData = (WeatherData)observable;
 float t = weatherData.getTemperature();
 float rh = weatherData.getHumidity();
 heatIndex = (float)
 (
 (16.923 + (0.185212 * t)) +
 (5.37941 * rh) -
 (0.100254 * t * rh) +
 (0.00941695 * (t * t)) +
 (0.00728898 * (rh * rh)) +
 (0.000345372 * (t * t * rh)) -
 (0.000814971 * (t * rh * rh)) +
 (0.0000102102 * (t * t * rh * rh)) -
 (0.000038646 * (t * t * t)) +
 (0.0000291583 * (rh * rh * rh)) +
 (0.00000142721 * (t * t * t * rh)) +
 (0.000000197483 * (t * rh * rh * rh)) -
 (0.0000000218429 * (t * t * t * rh * rh)) +
 (0.000000000843296 * (t * t * rh * rh * rh)) -
 (0.0000000000481975 * (t * t * t * rh * rh * rh)));
 display();
 }
 }
 public void display() {
 System.out.println("Heat index is " + heatIndex);
 }
}
 class StatisticsDisplay implements Observer, DisplayElement {
 private float maxTemp = 0.0f;
 private float minTemp = 200;
 private float tempSum= 0.0f; 
private int numReadings;
 public StatisticsDisplay(Observable observable) {
 observable.addObserver(this);
 }

 public void update(Observable observable, Object arg) {
 if (observable instanceof WeatherData) {
 WeatherData weatherData = (WeatherData)observable;
 float temp = weatherData.getTemperature();
 tempSum += temp;
 numReadings++;
 if (temp > maxTemp) {
 maxTemp = temp;
 }
 if (temp < minTemp) {
 minTemp = temp;
 }
 display();
 }
 }
 public void display() {
 System.out.println("Avg/Max/Min temperature = " + (tempSum / numReadings)+ "/"
+ maxTemp + "/" + minTemp);
 }
}

 class WeatherData extends Observable { 
private float temperature;
 private float humidity;
 private float pressure;
 public WeatherData() { }
 public void measurementsChanged() {
 setChanged();
 notifyObservers();
 }
 public void setMeasurements(float temperature, float humidity, float pressure) {
 this.temperature = temperature;
 this.humidity = humidity;
 this.pressure = pressure;
 measurementsChanged();
 }
 public float getTemperature() {
 return temperature;
 }
 public float getHumidity() {
 return humidity;
 }

 public float getPressure() {
 return pressure;
 }
}
public class Main {
 public static void main(String[] args) {
 WeatherData weatherData = new WeatherData(); 
CurrentConditionsDisplay currentConditions = new
CurrentConditionsDisplay(weatherData);
 StatisticsDisplay statisticsDisplay = new
StatisticsDisplay(weatherData);
 ForecastDisplay forecastDisplay = new ForecastDisplay(weatherData);
 weatherData.setMeasurements(80, 65, 30.4f);
 weatherData.setMeasurements(82, 70, 29.2f);
 weatherData.setMeasurements(78, 90, 29.2f);
 }
}


Q.2. Write a python program to implement Polynomial Linear Regression for given dataset
:
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

# Load the dataset
df = pd.read_csv("dataset.csv")

# Split the data into features (X) and target (y)
X = df.drop("target", axis=1)
y = df.target

# Split the data into training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Transform the training data to a higher-degree polynomial
poly = PolynomialFeatures(degree=2)
X_train_poly = poly.fit_transform(X_train)

# Create the model
model = LinearRegression()

# Train the model on the training data
model.fit(X_train_poly, y_train)

# Transform the test data to a higher-degree polynomial
X_test_poly = poly.transform(X_test)

# Make predictions on the test data
y_pred = model.predict(X_test_poly)

# Calculate the mean squared error
mse = mean_squared_error(y_test, y_pred)

# Print the results
print("Mean Squared Error:", mse)
print("Coefficients:", model.coef_)
print("Intercept:", model.intercept_)


q3)Create your Django app in which after running the server, you should see on the browser,
the text “Hello! I am learning Django”, which you defined in the index view.
:
django-admin startproject myproject
cd myproject
python manage.py startapp myapp
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello! I am learning Django")
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('myapp/', include('myapp.urls')),
    path('admin/', admin.site.urls),
]

