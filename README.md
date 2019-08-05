# TA-Lib Tutorial

A short tutorial on how to use the TA-Lib techincal analysis library.
https://www.ta-lib.org/


<div tabindex="-1" id="notebook" class="border-box-sizing">

<div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">


"TA-Lib is widely used by trading software developers requiring to perform technical analysis of financial market data."  
We will show how to use them here, in the context of pinkfish.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [1]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="o">%%</span><span class="nx">javascript</span>
<span class="nx">IPython</span><span class="p">.</span><span class="nx">OutputArea</span><span class="p">.</span><span class="nx">prototype</span><span class="p">.</span><span class="nx">_should_scroll</span> <span class="o">=</span> <span class="kd">function</span><span class="p">(</span><span class="nx">lines</span><span class="p">)</span> <span class="p">{</span>
    <span class="k">return</span> <span class="kc">false</span><span class="p">;</span>
<span class="p">}</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_javascript "><script type="text/javascript">var element = $('#c399dc74-0095-4d91-9ac4-e9ab290d5743'); IPython.OutputArea.prototype._should_scroll = function(lines) { return false; }</script></div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [2]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="c1"># use future imports for python 3.x forward compatibility</span>
<span class="kn">from</span> <span class="nn">__future__</span> <span class="kn">import</span> <span class="n">print_function</span>
<span class="kn">from</span> <span class="nn">__future__</span> <span class="kn">import</span> <span class="n">unicode_literals</span>
<span class="kn">from</span> <span class="nn">__future__</span> <span class="kn">import</span> <span class="n">division</span>
<span class="kn">from</span> <span class="nn">__future__</span> <span class="kn">import</span> <span class="n">absolute_import</span>

<span class="c1"># other imports</span>
<span class="kn">import</span> <span class="nn">pandas</span> <span class="kn">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="kn">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">datetime</span>
<span class="kn">import</span> <span class="nn">talib</span>
<span class="kn">from</span> <span class="nn">talib.abstract</span> <span class="kn">import</span> <span class="o">*</span>
<span class="kn">from</span> <span class="nn">talib</span> <span class="kn">import</span> <span class="n">MA_Type</span>
<span class="kn">import</span> <span class="nn">itable</span>

<span class="c1"># project imports</span>
<span class="kn">import</span> <span class="nn">pinkfish</span> <span class="kn">as</span> <span class="nn">pf</span>

<span class="c1"># format price data</span>
<span class="n">pd</span><span class="o">.</span><span class="n">options</span><span class="o">.</span><span class="n">display</span><span class="o">.</span><span class="n">float_format</span> <span class="o">=</span> <span class="s1">'{:0.2f}'</span><span class="o">.</span><span class="n">format</span>

<span class="o">%</span><span class="k">matplotlib</span> inline
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [3]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="c1"># set size of inline plots</span>
<span class="sd">'''note: rcParams can't be in same cell as import matplotlib</span>
 <span class="sd">or %matplotlib inline</span>
 <span class="sd"></span> 
 <span class="sd">%matplotlib notebook: will lead to interactive plots embedded within</span>
 <span class="sd">the notebook, you can zoom and resize the figure</span>
 <span class="sd"></span> 
 <span class="sd">%matplotlib inline: only draw static images in the notebook</span>
<span class="sd">'''</span>
<span class="n">plt</span><span class="o">.</span><span class="n">rcParams</span><span class="p">[</span><span class="s2">"figure.figsize"</span><span class="p">]</span> <span class="o">=</span> <span class="p">(</span><span class="mi">10</span><span class="p">,</span> <span class="mi">7</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Some global data

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [4]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">symbol</span> <span class="o">=</span> <span class="s1">'SPY'</span>
<span class="n">start</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="p">(</span><span class="mi">2018</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">end</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Fetch symbol data from cache, if available.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [5]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">ts</span> <span class="o">=</span> <span class="n">pf</span><span class="o">.</span><span class="n">fetch_timeseries</span><span class="p">(</span><span class="n">symbol</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [6]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">ts</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[6]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div><style scoped="">.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }</style>

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

</tr>

<tr>

<th>date</th>

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

<th>2019-07-30</th>

<td>301.17</td>

<td>299.49</td>

<td>299.91</td>

<td>300.72</td>

<td>45849000.00</td>

<td>300.72</td>

</tr>

<tr>

<th>2019-07-31</th>

<td>301.20</td>

<td>295.20</td>

<td>300.99</td>

<td>297.43</td>

<td>104245200.00</td>

<td>297.43</td>

</tr>

<tr>

<th>2019-08-01</th>

<td>300.87</td>

<td>293.96</td>

<td>297.60</td>

<td>294.84</td>

<td>142646600.00</td>

<td>294.84</td>

</tr>

<tr>

<th>2019-08-02</th>

<td>294.12</td>

<td>290.90</td>

<td>293.85</td>

<td>292.62</td>

<td>115917700.00</td>

<td>292.62</td>

</tr>

<tr>

<th>2019-08-05</th>

<td>288.21</td>

<td>284.81</td>

<td>288.09</td>

<td>284.81</td>

<td>64929017.00</td>

<td>284.81</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Select timeseries between start and end.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [7]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">ts</span> <span class="o">=</span> <span class="n">pf</span><span class="o">.</span><span class="n">select_tradeperiod</span><span class="p">(</span><span class="n">ts</span><span class="p">,</span> <span class="n">start</span><span class="p">,</span> <span class="n">end</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [8]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">ts</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[8]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div><style scoped="">.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }</style>

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

</tr>

<tr>

<th>date</th>

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

<th>2017-01-03</th>

<td>225.83</td>

<td>223.88</td>

<td>225.04</td>

<td>225.24</td>

<td>91366500.00</td>

<td>214.83</td>

</tr>

<tr>

<th>2017-01-04</th>

<td>226.75</td>

<td>225.61</td>

<td>225.62</td>

<td>226.58</td>

<td>78744400.00</td>

<td>216.11</td>

</tr>

<tr>

<th>2017-01-05</th>

<td>226.58</td>

<td>225.48</td>

<td>226.27</td>

<td>226.40</td>

<td>78379000.00</td>

<td>215.94</td>

</tr>

<tr>

<th>2017-01-06</th>

<td>227.75</td>

<td>225.90</td>

<td>226.53</td>

<td>227.21</td>

<td>71559900.00</td>

<td>216.71</td>

</tr>

<tr>

<th>2017-01-09</th>

<td>227.07</td>

<td>226.42</td>

<td>226.91</td>

<td>226.46</td>

<td>46939700.00</td>

<td>215.99</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Get info about TA-Lib[¶](#Get-info-about-TA-Lib)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [9]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="s1">'There are {} TA-Lib functions!'</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">talib</span><span class="o">.</span><span class="n">get_functions</span><span class="p">())))</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>There are 158 TA-Lib functions!
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Here is a complete listing of the functions by group:

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [10]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">for</span> <span class="n">group</span><span class="p">,</span> <span class="n">funcs</span> <span class="ow">in</span> <span class="n">talib</span><span class="o">.</span><span class="n">get_function_groups</span><span class="p">()</span><span class="o">.</span><span class="n">items</span><span class="p">():</span>
    <span class="k">print</span><span class="p">(</span><span class="n">group</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="s1">'-----------------------------------------'</span><span class="p">)</span>
    <span class="k">for</span> <span class="n">func</span> <span class="ow">in</span> <span class="n">funcs</span><span class="p">:</span>
        <span class="n">f</span> <span class="o">=</span> <span class="n">Function</span><span class="p">(</span><span class="n">func</span><span class="p">)</span>
        <span class="k">print</span><span class="p">(</span><span class="s1">'{} - {}'</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="n">func</span><span class="p">,</span> <span class="n">f</span><span class="o">.</span><span class="n">info</span><span class="p">[</span><span class="s1">'display_name'</span><span class="p">]))</span>
    <span class="k">print</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>Pattern Recognition
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

Volume Indicators
-----------------------------------------
AD - Chaikin A/D Line
ADOSC - Chaikin A/D Oscillator
OBV - On Balance Volume

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

Cycle Indicators
-----------------------------------------
HT_DCPERIOD - Hilbert Transform - Dominant Cycle Period
HT_DCPHASE - Hilbert Transform - Dominant Cycle Phase
HT_PHASOR - Hilbert Transform - Phasor Components
HT_SINE - Hilbert Transform - SineWave
HT_TRENDMODE - Hilbert Transform - Trend vs Cycle Mode

Volatility Indicators
-----------------------------------------
ATR - Average True Range
NATR - Normalized Average True Range
TRANGE - True Range

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

Price Transform
-----------------------------------------
AVGPRICE - Average Price
MEDPRICE - Median Price
TYPPRICE - Typical Price
WCLPRICE - Weighted Close Price

</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Get info about a specific TA-Lib function[¶](#Get-info-about-a-specific-TA-Lib-function)

There are 2 different API that are available with talib, namely Function API and Abstract API. For the Function API, you pass in a price series. For the Abstract API, you pass in a collection of named inputs: 'open', 'high', 'low', 'close', and 'volume'. One or more of these may be used as defaults, but can be changed with the 'price' parameter.

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Print the function instance to get documentation. We see that SMA has the parameter 'timeperiod' with default '30'. The input_arrays can be a dataframe with columns named 'open', 'high', 'low', 'close', and 'volume'.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [11]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">SMA</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>SMA([input_arrays], [timeperiod=30])

Simple Moving Average (Overlap Studies)

Inputs:
    price: (any ndarray)
Parameters:
    timeperiod: 30
Outputs:
    real
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

More information is available through the 'info' property. We observe here that the default price used is 'close'. This can be changed by setting 'price' in the function call, e.g. price='open'.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [12]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">SMA</span><span class="o">.</span><span class="n">info</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>{'input_names': OrderedDict([('price', 'close')]), 'display_name': 'Simple Moving Average', 'name': 'SMA', 'parameters': OrderedDict([('timeperiod', 30)]), 'output_flags': OrderedDict([('real', ['Line'])]), 'function_flags': ['Output scale same as input'], 'group': 'Overlap Studies', 'output_names': ['real']}
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

If we just want to see the inputs, we can print the input_names property.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [13]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">SMA</span><span class="o">.</span><span class="n">input_names</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('price', 'close')])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

If we just want to see the parameters, we can print the paramters property.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [14]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">SMA</span><span class="o">.</span><span class="n">parameters</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('timeperiod', 30)])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

If we just want to see the outputs, we can print the output_names property.

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [15]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">SMA</span><span class="o">.</span><span class="n">output_names</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>['real']
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Create a technical indicator using talib[¶](#Create-a-technical-indicator-using-talib)

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Create technical indicator: SMA (using defaults: timeperiod=30, price='close')

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [16]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">sma</span> <span class="o">=</span> <span class="n">SMA</span><span class="p">(</span><span class="n">ts</span><span class="p">)</span>
<span class="n">sma</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[16]:</div>

<div class="output_text output_subarea output_execute_result">

<pre>date
2019-07-30   297.15
2019-07-31   297.32
2019-08-01   297.38
2019-08-02   297.27
2019-08-05   296.96
dtype: float64</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Create technical indicator: SMA (using: timeperiod=200, price='close')

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [17]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">sma200</span> <span class="o">=</span> <span class="n">SMA</span><span class="p">(</span><span class="n">ts</span><span class="p">,</span> <span class="n">timeperiod</span><span class="o">=</span><span class="mi">200</span><span class="p">)</span>
<span class="n">sma200</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[17]:</div>

<div class="output_text output_subarea output_execute_result">

<pre>date
2019-07-30   278.38
2019-07-31   278.50
2019-08-01   278.60
2019-08-02   278.69
2019-08-05   278.71
dtype: float64</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Create technical indicator: SMA (using: timeperiod=200, price='open')

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [18]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">sma200</span> <span class="o">=</span> <span class="n">SMA</span><span class="p">(</span><span class="n">ts</span><span class="p">,</span> <span class="n">timeperiod</span><span class="o">=</span><span class="mi">200</span><span class="p">,</span> <span class="n">price</span><span class="o">=</span><span class="s1">'open'</span><span class="p">)</span>
<span class="n">sma200</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[18]:</div>

<div class="output_text output_subarea output_execute_result">

<pre>date
2019-07-30   278.36
2019-07-31   278.48
2019-08-01   278.58
2019-08-02   278.67
2019-08-05   278.73
dtype: float64</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Add a technical indicator to a pinkfish timeseries[¶](#Add-a-technical-indicator-to-a-pinkfish-timeseries)

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [19]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">ts</span><span class="p">[</span><span class="s1">'sma200'</span><span class="p">]</span> <span class="o">=</span> <span class="n">sma200</span>
<span class="n">ts</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[19]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div><style scoped="">.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }</style>

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

<th>2019-07-30</th>

<td>301.17</td>

<td>299.49</td>

<td>299.91</td>

<td>300.72</td>

<td>45849000.00</td>

<td>300.72</td>

<td>278.36</td>

</tr>

<tr>

<th>2019-07-31</th>

<td>301.20</td>

<td>295.20</td>

<td>300.99</td>

<td>297.43</td>

<td>104245200.00</td>

<td>297.43</td>

<td>278.48</td>

</tr>

<tr>

<th>2019-08-01</th>

<td>300.87</td>

<td>293.96</td>

<td>297.60</td>

<td>294.84</td>

<td>142646600.00</td>

<td>294.84</td>

<td>278.58</td>

</tr>

<tr>

<th>2019-08-02</th>

<td>294.12</td>

<td>290.90</td>

<td>293.85</td>

<td>292.62</td>

<td>115917700.00</td>

<td>292.62</td>

<td>278.67</td>

</tr>

<tr>

<th>2019-08-05</th>

<td>288.21</td>

<td>284.81</td>

<td>288.09</td>

<td>284.81</td>

<td>64929017.00</td>

<td>284.81</td>

<td>278.73</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Try another one[¶](#Try-another-one)

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Commodity Channel Index

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [20]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">CCI</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>CCI([input_arrays], [timeperiod=14])

Commodity Channel Index (Momentum Indicators)

Inputs:
    prices: ['high', 'low', 'close']
Parameters:
    timeperiod: 14
Outputs:
    real
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [21]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">CCI</span><span class="o">.</span><span class="n">input_names</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('prices', ['high', 'low', 'close'])])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [22]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">CCI</span><span class="o">.</span><span class="n">parameters</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('timeperiod', 14)])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [23]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">cci</span> <span class="o">=</span> <span class="n">CCI</span><span class="p">(</span><span class="n">ts</span><span class="p">)</span>
<span class="n">ts</span><span class="p">[</span><span class="s1">'cci'</span><span class="p">]</span> <span class="o">=</span> <span class="n">cci</span>
<span class="n">ts</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[23]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div><style scoped="">.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }</style>

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

<th>2019-07-30</th>

<td>301.17</td>

<td>299.49</td>

<td>299.91</td>

<td>300.72</td>

<td>45849000.00</td>

<td>300.72</td>

<td>278.36</td>

<td>45.99</td>

</tr>

<tr>

<th>2019-07-31</th>

<td>301.20</td>

<td>295.20</td>

<td>300.99</td>

<td>297.43</td>

<td>104245200.00</td>

<td>297.43</td>

<td>278.48</td>

<td>-100.50</td>

</tr>

<tr>

<th>2019-08-01</th>

<td>300.87</td>

<td>293.96</td>

<td>297.60</td>

<td>294.84</td>

<td>142646600.00</td>

<td>294.84</td>

<td>278.58</td>

<td>-143.30</td>

</tr>

<tr>

<th>2019-08-02</th>

<td>294.12</td>

<td>290.90</td>

<td>293.85</td>

<td>292.62</td>

<td>115917700.00</td>

<td>292.62</td>

<td>278.67</td>

<td>-243.46</td>

</tr>

<tr>

<th>2019-08-05</th>

<td>288.21</td>

<td>284.81</td>

<td>288.09</td>

<td>284.81</td>

<td>64929017.00</td>

<td>284.81</td>

<td>278.73</td>

<td>-301.41</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

### Now for something a little more difficult[¶](#Now-for-something-a-little-more-difficult)

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Bollinger Bands

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [24]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">BBANDS</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>BBANDS([input_arrays], [timeperiod=5], [nbdevup=2], [nbdevdn=2], [matype=0])

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
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [25]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">BBANDS</span><span class="o">.</span><span class="n">input_names</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('price', 'close')])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [26]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">BBANDS</span><span class="o">.</span><span class="n">parameters</span><span class="p">)</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>OrderedDict([('timeperiod', 5), ('nbdevup', 2), ('nbdevdn', 2), ('matype', 0)])
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Print the available moving average types

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [27]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">attributes</span> <span class="o">=</span> <span class="p">[</span><span class="n">attr</span> <span class="k">for</span> <span class="n">attr</span> <span class="ow">in</span> <span class="nb">dir</span><span class="p">(</span><span class="n">MA_Type</span><span class="p">)</span> 
              <span class="k">if</span> <span class="ow">not</span> <span class="n">attr</span><span class="o">.</span><span class="n">startswith</span><span class="p">(</span><span class="s1">'__'</span><span class="p">)]</span>
<span class="n">attributes</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[27]:</div>

<div class="output_text output_subarea output_execute_result">

<pre>['DEMA', 'EMA', 'KAMA', 'MAMA', 'SMA', 'T3', 'TEMA', 'TRIMA', 'WMA', '_lookup']</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [28]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="n">MA_Type</span><span class="o">.</span><span class="vm">__dict__</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[28]:</div>

<div class="output_text output_subarea output_execute_result">

<pre>{'_lookup': {0: 'Simple Moving Average',
  1: 'Exponential Moving Average',
  2: 'Weighted Moving Average',
  3: 'Double Exponential Moving Average',
  4: 'Triple Exponential Moving Average',
  5: 'Triangular Moving Average',
  6: 'Kaufman Adaptive Moving Average',
  7: 'MESA Adaptive Moving Average',
  8: 'Triple Generalized Double Exponential Moving Average'}}</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [29]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="k">print</span><span class="p">(</span><span class="n">MA_Type</span><span class="p">[</span><span class="n">MA_Type</span><span class="o">.</span><span class="n">DEMA</span><span class="p">])</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">

<pre>Double Exponential Moving Average
</pre>

</div>

</div>

</div>

</div>

</div>

<div class="cell border-box-sizing text_cell rendered">

<div class="inner_cell">

<div class="text_cell_render border-box-sizing rendered_html">

Set timeperiod=20 and matype=MA_Type.EMA

</div>

</div>

</div>

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">In [30]:</div>

<div class="inner_cell">

<div class="input_area">

<div class=" highlight hl-ipython2">

<pre><span></span><span class="c1">#upper, middle, lower = BBANDS(ts, timeperiod=20, matype=MA_Type.EMA)</span>
<span class="c1">#(for some reason, the abstract API doesn't work for BBANDS, so use the function API)</span>

<span class="n">upper</span><span class="p">,</span> <span class="n">middle</span><span class="p">,</span> <span class="n">lower</span> <span class="o">=</span> <span class="n">talib</span><span class="o">.</span><span class="n">BBANDS</span><span class="p">(</span><span class="n">ts</span><span class="o">.</span><span class="n">close</span><span class="p">,</span> <span class="n">timeperiod</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span> <span class="n">matype</span><span class="o">=</span><span class="n">MA_Type</span><span class="o">.</span><span class="n">EMA</span><span class="p">)</span>
<span class="n">ts</span><span class="p">[</span><span class="s1">'upper'</span><span class="p">]</span> <span class="o">=</span> <span class="n">upper</span><span class="p">;</span> <span class="n">ts</span><span class="p">[</span><span class="s1">'middle'</span><span class="p">]</span> <span class="o">=</span> <span class="n">middle</span><span class="p">;</span> <span class="n">ts</span><span class="p">[</span><span class="s1">'lower'</span><span class="p">]</span> <span class="o">=</span> <span class="n">lower</span>
<span class="n">ts</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
</pre>

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">Out[30]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">

<div><style scoped="">.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }</style>

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

<th>2019-07-30</th>

<td>301.17</td>

<td>299.49</td>

<td>299.91</td>

<td>300.72</td>

<td>45849000.00</td>

<td>300.72</td>

<td>278.36</td>

<td>45.99</td>

<td>301.79</td>

<td>298.53</td>

<td>295.28</td>

</tr>

<tr>

<th>2019-07-31</th>

<td>301.20</td>

<td>295.20</td>

<td>300.99</td>

<td>297.43</td>

<td>104245200.00</td>

<td>297.43</td>

<td>278.48</td>

<td>-100.50</td>

<td>301.54</td>

<td>298.43</td>

<td>295.32</td>

</tr>

<tr>

<th>2019-08-01</th>

<td>300.87</td>

<td>293.96</td>

<td>297.60</td>

<td>294.84</td>

<td>142646600.00</td>

<td>294.84</td>

<td>278.58</td>

<td>-143.30</td>

<td>301.75</td>

<td>298.09</td>

<td>294.43</td>

</tr>

<tr>

<th>2019-08-02</th>

<td>294.12</td>

<td>290.90</td>

<td>293.85</td>

<td>292.62</td>

<td>115917700.00</td>

<td>292.62</td>

<td>278.67</td>

<td>-243.46</td>

<td>302.18</td>

<td>297.57</td>

<td>292.96</td>

</tr>

<tr>

<th>2019-08-05</th>

<td>288.21</td>

<td>284.81</td>

<td>288.09</td>

<td>284.81</td>

<td>64929017.00</td>

<td>284.81</td>

<td>278.73</td>

<td>-301.41</td>

<td>303.97</td>

<td>296.35</td>

<td>288.73</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

</div>

</div>

</div>

</div>
