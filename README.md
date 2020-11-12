

![logo](https://i.postimg.cc/rmQyz7bT/png.png)


## Time Series Extract Library (tsExtract)


This library provides helper functions for preprocessing Time Series data for training Machine Learning models. Using sliding windows, tsExtract allows for the conversion of time series data to a form that can be fed into standard machine learning regression algorithms like Linear Regression, Decision Trees Regression and as well as Deep Learning. 

|![license_badge](https://img.shields.io/badge/LICENSE-GNU_GPL-BLACK) | ![stable image description here](https://img.shields.io/badge/STATUS-stable-green) | ![stable image description here](https://img.shields.io/badge/pypi-v1.0.0-yellow) | ![stable image description here](https://img.shields.io/badge/Anaconda-v1.0.0-blue) |
|--|--|--|--|


# Installation

<code> pip </code>

> **pip install tsextract**

<code> conda </code>
> **conda install tsextract**



## Main Features

* Take sliding window of data and with that, create additional columns representing the window. 
* Perform differencing on windowed data to remove stationarity. 
* Calculate statistics on windowed and differenced data. These include temporal and spectral statistics functions. 
* Plot visualisations. These include - 
* * Actual vs Predicted line and scatter plots
* * Lag correlation

## Usage

```python
print(df.head())
```


|                |Date                          |DAYTON_MW                         |
|----------------|-------------------------------|-----------------------------|
| |`2004-12-31 01:00:00`            |`1596.0`            |
|          |`2004-12-31 02:00:00` | `1517.0` |
|          |`2004-12-31 03:00:00`|`1486.0`|
| | `2004-12-31 04:00:00`|`1469.0` |
| |`2004-12-31 05:00:00` | `1472.0` |



Using the main **build_features** function


**build_features** takes in 4 arguments - 
* **Data**: Time series data in 1d. 

* **Request Dictionary**: Dictionary with the function type and parameters
* **Include_tzero** (optional) - This gives the option on whether to also include the column t+0. Can be quite handy when implementing difference networks. 
* **target_lag** - Sets lag value. If predicting 10 hours into the future, then a value of 10 should be included. Default is 3. 

```python
from tsextract.feature_extraction.extract import build_features

features_request = {
    "window":[10]
}

features = build_features(df["DAYTON_MW"], features_request, include_tzero=False)
```

The example above sends in a request for a sliding window size of 10. What is returned is a dataframe with 10 columns equal to the window size passed in. The final column is the target column with values shifted 3 time steps in the future. 


![enter image description here](https://i.postimg.cc/SRQTtbnH/Screenshot-2020-11-11-at-00-12-11.png)


### Features

* **window**: Performs windowing of the data. Parameter(s) passed in as a list. A single value will take a sliding window corresponding to that value. A parameter of 10 will take windows from 1 to 10. If [5, 10] is passed in instead, then a window of 5 - 10 will be taken instead. 

* **window_statistic**: This performs windowing like above, but then applies specified statistic operation to reduce matrix to a vector of 1d. 

* **difference/momentum/force**: Performs differencing by subtracting from the value in the present time step, the value in the previous time step. The parameter expected is a list of size 2 or 3. Just like in windowing, the first value refers to the window size. Two windowing values may also be passed in for windows in that range. 
The final value is the lag, this refers to the differencing lag for subtraction. A difference lag of 1 means values are subtracted from immediate past values (t3-t2, t2-t1, t1-t0 e.t.c) while a difference lag of 3 will subtract from 3 time steps before (t6-t3, t5-t2, t4-t1 e.t.c).
Momentum & Force are 2nd & 3rd order differences. 

* **difference_statistic/momentum_statistic/force_statistic**: Similarly, this performs the operations described above, but then applies the specified statistic. 

```python
from tsextract.feature_extraction.extract import build_features
from tsextract.domain.statistics import median, mean, skew, kurtosis
from tsextract.domain.temporal import abs_energy

features_request = {
    "window":[10], 
    "window_statistic":[24, median], 
    "difference":[15, 10],
    "momentum":[15, 10],
    "force":[15, 10],
    "difference_statistic":[15, 10, mean], 
    "momentum_statistic":[15, 10, abs_energy],
    "force_statistic":[15, 10, mean], 
}

features = build_features(df["DAYTON_MW"], features_request, include_tzero=True, target_lag=3)
```

![enter image description here](https://i.postimg.cc/VvVhrsgm/Screenshot-2020-11-11-at-01-00-16.png)

## Dependencies



## License

[GNU GPL V3](http://www.gnu.org/licenses/quick-guide-gplv3.html)


# Contribute

Contributors of all experience levels are welcome. Please see the contributing guide. 


### Source Code

<code> You can get the latest source code </code>

> git clone https://github.com/cydal/tsExtract.git 





