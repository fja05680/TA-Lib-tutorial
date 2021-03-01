# TA-Lib Tutorial

"TA-Lib is widely used by trading software developers requiring to perform technical analysis of financial market data."


```javascript
%%javascript
IPython.OutputArea.prototype._should_scroll = function(lines) {
    return false;
}
```


    <IPython.core.display.Javascript object>



```python
# imports
import pandas as pd
import pandas_datareader.data as pdr
import datetime
import talib
from talib.abstract import *
from talib import MA_Type

# format price data
pd.options.display.float_format = '{:0.2f}'.format
```

Some global data


```python
symbol = 'SPY'
start = datetime.datetime(2018, 1, 1)
end = datetime.datetime.now()
```

Fetch timeseries


```python
ts = pdr.DataReader(symbol, 'yahoo', start, end)
ts.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>High</th>
      <th>Low</th>
      <th>Open</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-02-22</th>
      <td>389.62</td>
      <td>386.74</td>
      <td>387.06</td>
      <td>387.03</td>
      <td>67160000.00</td>
      <td>387.03</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>388.95</td>
      <td>380.20</td>
      <td>384.66</td>
      <td>387.50</td>
      <td>106997200.00</td>
      <td>387.50</td>
    </tr>
    <tr>
      <th>2021-02-24</th>
      <td>392.23</td>
      <td>385.27</td>
      <td>386.33</td>
      <td>391.77</td>
      <td>72226600.00</td>
      <td>391.77</td>
    </tr>
    <tr>
      <th>2021-02-25</th>
      <td>391.88</td>
      <td>380.78</td>
      <td>390.41</td>
      <td>382.33</td>
      <td>146086500.00</td>
      <td>382.33</td>
    </tr>
    <tr>
      <th>2021-02-26</th>
      <td>385.58</td>
      <td>378.23</td>
      <td>384.35</td>
      <td>380.36</td>
      <td>152534900.00</td>
      <td>380.36</td>
    </tr>
  </tbody>
</table>
</div>




```python
def _adj_column_names(ts):
    """
    ta-lib expects columns to be lower case; to be consistent,
    change date index
    """
    ts.columns = [col.lower().replace(' ','_') for col in ts.columns]
    ts.index.names = ['date']
    return ts

ts = _adj_column_names(ts)
```

Select timeseries between start and end.

### Get info about TA-Lib


```python
print('There are {} TA-Lib functions!'.format(len(talib.get_functions())))
```

    There are 158 TA-Lib functions!


Here is a complete listing of the functions by group:


```python
for group, funcs in talib.get_function_groups().items():
    print(group)
    print('-----------------------------------------')
    for func in funcs:
        f = Function(func)
        print('{} - {}'.format(func, f.info['display_name']))
    print()
```

    Cycle Indicators
    -----------------------------------------
    HT_DCPERIOD - Hilbert Transform - Dominant Cycle Period
    HT_DCPHASE - Hilbert Transform - Dominant Cycle Phase
    HT_PHASOR - Hilbert Transform - Phasor Components
    HT_SINE - Hilbert Transform - SineWave
    HT_TRENDMODE - Hilbert Transform - Trend vs Cycle Mode
    
    Math Operators
    -----------------------------------------
    ADD - Vector Arithmetic Add
    DIV - Vector Arithmetic Div
    MAX - Highest value over a specified period
    MAXINDEX - Index of highest value over a specified period
    MIN - Lowest value over a specified period
    MININDEX - Index of lowest value over a specified period
    MINMAX - Lowest and highest values over a specified period
    MINMAXINDEX - Indexes of lowest and highest values over a specified period
    MULT - Vector Arithmetic Mult
    SUB - Vector Arithmetic Substraction
    SUM - Summation
    
    Math Transform
    -----------------------------------------
    ACOS - Vector Trigonometric ACos
    ASIN - Vector Trigonometric ASin
    ATAN - Vector Trigonometric ATan
    CEIL - Vector Ceil
    COS - Vector Trigonometric Cos
    COSH - Vector Trigonometric Cosh
    EXP - Vector Arithmetic Exp
    FLOOR - Vector Floor
    LN - Vector Log Natural
    LOG10 - Vector Log10
    SIN - Vector Trigonometric Sin
    SINH - Vector Trigonometric Sinh
    SQRT - Vector Square Root
    TAN - Vector Trigonometric Tan
    TANH - Vector Trigonometric Tanh
    
    Momentum Indicators
    -----------------------------------------
    ADX - Average Directional Movement Index
    ADXR - Average Directional Movement Index Rating
    APO - Absolute Price Oscillator
    AROON - Aroon
    AROONOSC - Aroon Oscillator
    BOP - Balance Of Power
    CCI - Commodity Channel Index
    CMO - Chande Momentum Oscillator
    DX - Directional Movement Index
    MACD - Moving Average Convergence/Divergence
    MACDEXT - MACD with controllable MA type
    MACDFIX - Moving Average Convergence/Divergence Fix 12/26
    MFI - Money Flow Index
    MINUS_DI - Minus Directional Indicator
    MINUS_DM - Minus Directional Movement
    MOM - Momentum
    PLUS_DI - Plus Directional Indicator
    PLUS_DM - Plus Directional Movement
    PPO - Percentage Price Oscillator
    ROC - Rate of change : ((price/prevPrice)-1)*100
    ROCP - Rate of change Percentage: (price-prevPrice)/prevPrice
    ROCR - Rate of change ratio: (price/prevPrice)
    ROCR100 - Rate of change ratio 100 scale: (price/prevPrice)*100
    RSI - Relative Strength Index
    STOCH - Stochastic
    STOCHF - Stochastic Fast
    STOCHRSI - Stochastic Relative Strength Index
    TRIX - 1-day Rate-Of-Change (ROC) of a Triple Smooth EMA
    ULTOSC - Ultimate Oscillator
    WILLR - Williams' %R
    
    Overlap Studies
    -----------------------------------------
    BBANDS - Bollinger Bands
    DEMA - Double Exponential Moving Average
    EMA - Exponential Moving Average
    HT_TRENDLINE - Hilbert Transform - Instantaneous Trendline
    KAMA - Kaufman Adaptive Moving Average
    MA - Moving average
    MAMA - MESA Adaptive Moving Average
    MAVP - Moving average with variable period
    MIDPOINT - MidPoint over period
    MIDPRICE - Midpoint Price over period
    SAR - Parabolic SAR
    SAREXT - Parabolic SAR - Extended
    SMA - Simple Moving Average
    T3 - Triple Exponential Moving Average (T3)
    TEMA - Triple Exponential Moving Average
    TRIMA - Triangular Moving Average
    WMA - Weighted Moving Average
    
    Pattern Recognition
    -----------------------------------------
    CDL2CROWS - Two Crows
    CDL3BLACKCROWS - Three Black Crows
    CDL3INSIDE - Three Inside Up/Down
    CDL3LINESTRIKE - Three-Line Strike 
    CDL3OUTSIDE - Three Outside Up/Down
    CDL3STARSINSOUTH - Three Stars In The South
    CDL3WHITESOLDIERS - Three Advancing White Soldiers
    CDLABANDONEDBABY - Abandoned Baby
    CDLADVANCEBLOCK - Advance Block
    CDLBELTHOLD - Belt-hold
    CDLBREAKAWAY - Breakaway
    CDLCLOSINGMARUBOZU - Closing Marubozu
    CDLCONCEALBABYSWALL - Concealing Baby Swallow
    CDLCOUNTERATTACK - Counterattack
    CDLDARKCLOUDCOVER - Dark Cloud Cover
    CDLDOJI - Doji
    CDLDOJISTAR - Doji Star
    CDLDRAGONFLYDOJI - Dragonfly Doji
    CDLENGULFING - Engulfing Pattern
    CDLEVENINGDOJISTAR - Evening Doji Star
    CDLEVENINGSTAR - Evening Star
    CDLGAPSIDESIDEWHITE - Up/Down-gap side-by-side white lines
    CDLGRAVESTONEDOJI - Gravestone Doji
    CDLHAMMER - Hammer
    CDLHANGINGMAN - Hanging Man
    CDLHARAMI - Harami Pattern
    CDLHARAMICROSS - Harami Cross Pattern
    CDLHIGHWAVE - High-Wave Candle
    CDLHIKKAKE - Hikkake Pattern
    CDLHIKKAKEMOD - Modified Hikkake Pattern
    CDLHOMINGPIGEON - Homing Pigeon
    CDLIDENTICAL3CROWS - Identical Three Crows
    CDLINNECK - In-Neck Pattern
    CDLINVERTEDHAMMER - Inverted Hammer
    CDLKICKING - Kicking
    CDLKICKINGBYLENGTH - Kicking - bull/bear determined by the longer marubozu
    CDLLADDERBOTTOM - Ladder Bottom
    CDLLONGLEGGEDDOJI - Long Legged Doji
    CDLLONGLINE - Long Line Candle
    CDLMARUBOZU - Marubozu
    CDLMATCHINGLOW - Matching Low
    CDLMATHOLD - Mat Hold
    CDLMORNINGDOJISTAR - Morning Doji Star
    CDLMORNINGSTAR - Morning Star
    CDLONNECK - On-Neck Pattern
    CDLPIERCING - Piercing Pattern
    CDLRICKSHAWMAN - Rickshaw Man
    CDLRISEFALL3METHODS - Rising/Falling Three Methods
    CDLSEPARATINGLINES - Separating Lines
    CDLSHOOTINGSTAR - Shooting Star
    CDLSHORTLINE - Short Line Candle
    CDLSPINNINGTOP - Spinning Top
    CDLSTALLEDPATTERN - Stalled Pattern
    CDLSTICKSANDWICH - Stick Sandwich
    CDLTAKURI - Takuri (Dragonfly Doji with very long lower shadow)
    CDLTASUKIGAP - Tasuki Gap
    CDLTHRUSTING - Thrusting Pattern
    CDLTRISTAR - Tristar Pattern
    CDLUNIQUE3RIVER - Unique 3 River
    CDLUPSIDEGAP2CROWS - Upside Gap Two Crows
    CDLXSIDEGAP3METHODS - Upside/Downside Gap Three Methods
    
    Price Transform
    -----------------------------------------
    AVGPRICE - Average Price
    MEDPRICE - Median Price
    TYPPRICE - Typical Price
    WCLPRICE - Weighted Close Price
    
    Statistic Functions
    -----------------------------------------
    BETA - Beta
    CORREL - Pearson's Correlation Coefficient (r)
    LINEARREG - Linear Regression
    LINEARREG_ANGLE - Linear Regression Angle
    LINEARREG_INTERCEPT - Linear Regression Intercept
    LINEARREG_SLOPE - Linear Regression Slope
    STDDEV - Standard Deviation
    TSF - Time Series Forecast
    VAR - Variance
    
    Volatility Indicators
    -----------------------------------------
    ATR - Average True Range
    NATR - Normalized Average True Range
    TRANGE - True Range
    
    Volume Indicators
    -----------------------------------------
    AD - Chaikin A/D Line
    ADOSC - Chaikin A/D Oscillator
    OBV - On Balance Volume
    


### Get info about a specific TA-Lib function

There are 2 different API that are available with talib, namely Function API and Abstract API.  For the Function API, you pass in a price series.  For the Abstract API, you pass in a collection of named inputs: 'open', 'high', 'low', 'close', and 'volume'.  One or more of these may be used as defaults, but can be changed with the 'price' parameter.  

Print the function instance to get documentation.  We see that SMA has the parameter 'timeperiod' with default '30'.  The input_arrays can be a dataframe with columns named 'open', 'high', 'low', 'close', and 'volume'.


```python
print(SMA)
```

    SMA([input_arrays], [timeperiod=30])
    
    Simple Moving Average (Overlap Studies)
    
    Inputs:
        price: (any ndarray)
    Parameters:
        timeperiod: 30
    Outputs:
        real


More information is available through the 'info' property.  We observe here that the default price used is 'close'.  This can be changed by setting 'price' in the function call, e.g. price='open'.


```python
print(SMA.info)
```

    {'name': 'SMA', 'group': 'Overlap Studies', 'display_name': 'Simple Moving Average', 'function_flags': ['Output scale same as input'], 'input_names': OrderedDict([('price', 'close')]), 'parameters': OrderedDict([('timeperiod', 30)]), 'output_flags': OrderedDict([('real', ['Line'])]), 'output_names': ['real']}


If we just want to see the inputs, we can print the input_names property.


```python
print(SMA.input_names)
```

    OrderedDict([('price', 'close')])


If we just want to see the parameters, we can print the paramters property.


```python
print(SMA.parameters)
```

    OrderedDict([('timeperiod', 30)])


If we just want to see the outputs, we can print the output_names property.


```python
print(SMA.output_names)
```

    ['real']


### Create a technical indicator using talib

Create technical indicator: SMA (using defaults: timeperiod=30, price='close')


```python
sma = SMA(ts)
sma.tail()
```




    date
    2021-02-22   383.76
    2021-02-23   383.97
    2021-02-24   384.40
    2021-02-25   384.52
    2021-02-26   384.54
    dtype: float64



Create technical indicator: SMA (using: timeperiod=200, price='close')


```python
sma200 = SMA(ts, timeperiod=200)
sma200.tail()
```




    date
    2021-02-22   342.20
    2021-02-23   342.70
    2021-02-24   343.20
    2021-02-25   343.65
    2021-02-26   344.12
    dtype: float64



Create technical indicator: SMA (using: timeperiod=200, price='open')


```python
sma200 = SMA(ts, timeperiod=200, price='open')
sma200.tail()
```




    date
    2021-02-22   342.24
    2021-02-23   342.73
    2021-02-24   343.20
    2021-02-25   343.70
    2021-02-26   344.16
    dtype: float64



### Add a technical indicator to a pinkfish timeseries


```python
ts['sma200'] = sma200
ts.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>close</th>
      <th>volume</th>
      <th>adj_close</th>
      <th>sma200</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-02-22</th>
      <td>389.62</td>
      <td>386.74</td>
      <td>387.06</td>
      <td>387.03</td>
      <td>67160000.00</td>
      <td>387.03</td>
      <td>342.24</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>388.95</td>
      <td>380.20</td>
      <td>384.66</td>
      <td>387.50</td>
      <td>106997200.00</td>
      <td>387.50</td>
      <td>342.73</td>
    </tr>
    <tr>
      <th>2021-02-24</th>
      <td>392.23</td>
      <td>385.27</td>
      <td>386.33</td>
      <td>391.77</td>
      <td>72226600.00</td>
      <td>391.77</td>
      <td>343.20</td>
    </tr>
    <tr>
      <th>2021-02-25</th>
      <td>391.88</td>
      <td>380.78</td>
      <td>390.41</td>
      <td>382.33</td>
      <td>146086500.00</td>
      <td>382.33</td>
      <td>343.70</td>
    </tr>
    <tr>
      <th>2021-02-26</th>
      <td>385.58</td>
      <td>378.23</td>
      <td>384.35</td>
      <td>380.36</td>
      <td>152534900.00</td>
      <td>380.36</td>
      <td>344.16</td>
    </tr>
  </tbody>
</table>
</div>



### Try another one

Commodity Channel Index


```python
print(CCI)
```

    CCI([input_arrays], [timeperiod=14])
    
    Commodity Channel Index (Momentum Indicators)
    
    Inputs:
        prices: ['high', 'low', 'close']
    Parameters:
        timeperiod: 14
    Outputs:
        real



```python
print(CCI.input_names)
```

    OrderedDict([('prices', ['high', 'low', 'close'])])



```python
print(CCI.parameters)
```

    OrderedDict([('timeperiod', 14)])



```python
cci = CCI(ts)
ts['cci'] = cci
ts.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>close</th>
      <th>volume</th>
      <th>adj_close</th>
      <th>sma200</th>
      <th>cci</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-02-22</th>
      <td>389.62</td>
      <td>386.74</td>
      <td>387.06</td>
      <td>387.03</td>
      <td>67160000.00</td>
      <td>387.03</td>
      <td>342.24</td>
      <td>-16.31</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>388.95</td>
      <td>380.20</td>
      <td>384.66</td>
      <td>387.50</td>
      <td>106997200.00</td>
      <td>387.50</td>
      <td>342.73</td>
      <td>-92.48</td>
    </tr>
    <tr>
      <th>2021-02-24</th>
      <td>392.23</td>
      <td>385.27</td>
      <td>386.33</td>
      <td>391.77</td>
      <td>72226600.00</td>
      <td>391.77</td>
      <td>343.20</td>
      <td>12.88</td>
    </tr>
    <tr>
      <th>2021-02-25</th>
      <td>391.88</td>
      <td>380.78</td>
      <td>390.41</td>
      <td>382.33</td>
      <td>146086500.00</td>
      <td>382.33</td>
      <td>343.70</td>
      <td>-173.20</td>
    </tr>
    <tr>
      <th>2021-02-26</th>
      <td>385.58</td>
      <td>378.23</td>
      <td>384.35</td>
      <td>380.36</td>
      <td>152534900.00</td>
      <td>380.36</td>
      <td>344.16</td>
      <td>-218.21</td>
    </tr>
  </tbody>
</table>
</div>



### Now for something a little more difficult

Bollinger Bands


```python
print(BBANDS)
```

    BBANDS([input_arrays], [timeperiod=5], [nbdevup=2], [nbdevdn=2], [matype=0])
    
    Bollinger Bands (Overlap Studies)
    
    Inputs:
        price: (any ndarray)
    Parameters:
        timeperiod: 5
        nbdevup: 2
        nbdevdn: 2
        matype: 0 (Simple Moving Average)
    Outputs:
        upperband
        middleband
        lowerband



```python
print(BBANDS.input_names)
```

    OrderedDict([('price', 'close')])



```python
print(BBANDS.parameters)
```

    OrderedDict([('timeperiod', 5), ('nbdevup', 2), ('nbdevdn', 2), ('matype', 0)])


Print the available moving average types


```python
attributes = [attr for attr in dir(MA_Type) 
              if not attr.startswith('__')]
attributes
```




    ['DEMA', 'EMA', 'KAMA', 'MAMA', 'SMA', 'T3', 'TEMA', 'TRIMA', 'WMA', '_lookup']




```python
MA_Type.__dict__
```




    {'_lookup': {0: 'Simple Moving Average',
      1: 'Exponential Moving Average',
      2: 'Weighted Moving Average',
      3: 'Double Exponential Moving Average',
      4: 'Triple Exponential Moving Average',
      5: 'Triangular Moving Average',
      6: 'Kaufman Adaptive Moving Average',
      7: 'MESA Adaptive Moving Average',
      8: 'Triple Generalized Double Exponential Moving Average'}}




```python
print(MA_Type[MA_Type.DEMA])
```

    Double Exponential Moving Average


Set timeperiod=20 and matype=MA_Type.EMA


```python
#upper, middle, lower = BBANDS(ts, timeperiod=20, matype=MA_Type.EMA)
#(for some reason, the abstract API doesn't work for BBANDS, so use the function API)

upper, middle, lower = talib.BBANDS(ts.close, timeperiod=20, matype=MA_Type.EMA)
ts['upper'] = upper; ts['middle'] = middle; ts['lower'] = lower
ts.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>high</th>
      <th>low</th>
      <th>open</th>
      <th>close</th>
      <th>volume</th>
      <th>adj_close</th>
      <th>sma200</th>
      <th>cci</th>
      <th>upper</th>
      <th>middle</th>
      <th>lower</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-02-22</th>
      <td>389.62</td>
      <td>386.74</td>
      <td>387.06</td>
      <td>387.03</td>
      <td>67160000.00</td>
      <td>387.03</td>
      <td>342.24</td>
      <td>-16.31</td>
      <td>399.38</td>
      <td>386.44</td>
      <td>373.49</td>
    </tr>
    <tr>
      <th>2021-02-23</th>
      <td>388.95</td>
      <td>380.20</td>
      <td>384.66</td>
      <td>387.50</td>
      <td>106997200.00</td>
      <td>387.50</td>
      <td>342.73</td>
      <td>-92.48</td>
      <td>399.50</td>
      <td>386.54</td>
      <td>373.58</td>
    </tr>
    <tr>
      <th>2021-02-24</th>
      <td>392.23</td>
      <td>385.27</td>
      <td>386.33</td>
      <td>391.77</td>
      <td>72226600.00</td>
      <td>391.77</td>
      <td>343.20</td>
      <td>12.88</td>
      <td>400.23</td>
      <td>387.04</td>
      <td>373.84</td>
    </tr>
    <tr>
      <th>2021-02-25</th>
      <td>391.88</td>
      <td>380.78</td>
      <td>390.41</td>
      <td>382.33</td>
      <td>146086500.00</td>
      <td>382.33</td>
      <td>343.70</td>
      <td>-173.20</td>
      <td>398.80</td>
      <td>386.59</td>
      <td>374.38</td>
    </tr>
    <tr>
      <th>2021-02-26</th>
      <td>385.58</td>
      <td>378.23</td>
      <td>384.35</td>
      <td>380.36</td>
      <td>152534900.00</td>
      <td>380.36</td>
      <td>344.16</td>
      <td>-218.21</td>
      <td>397.86</td>
      <td>386.00</td>
      <td>374.13</td>
    </tr>
  </tbody>
</table>
</div>


