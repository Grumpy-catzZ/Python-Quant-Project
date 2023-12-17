# Python-Quant-Project
Submission has 2 notebooks

data_download notebook contains code on downloading data and exploring a bit of it.

submission notebook contains all main code for features and backtesting. Its structured as follows

- Feature 1 ( Stochastic Oscillator) - design, backtest and evaluate 
- Common Backestesting Code for Feature 2, 3, 4 and 5
- Common Code for Periodic Returns
- Feature 2 ( Mean +/- 2 Std Dev Bands )  - design, backtest and evaluate
- Feature 3 ( Bands around VWAP) - design, backtest and evaluate
- Feature 4 (Harmonic Mean) - design, backtest and evaluate
- Feature 5 (Bands around exponential moving average) - design, backtest and evaluate

  NOTE - The data file was larger than what github accepts. Kindly download it from here https://drive.google.com/file/d/1ZanTWpVlG7YaOm_wRv_fPvrs46SaWE_I/view?usp=sharing


UPDATES: 
Change the for loop with vectorized statements to reduce the time significantly: ( where we create our feature variables )

```
temp['L14'] = temp.groupby('code')['low'].rolling(price_window).min().reset_index(level=0, drop=True)
temp['H14'] = temp.groupby('code')['high'].rolling(price_window).max().reset_index(level=0, drop = True)

def calculate_K(group):
    return (group['close'] - group['L14'])/(group['H14'] - group['L14'] )*100
temp['%k'] = temp.groupby('code').apply(calculate_K).reset_index(level = 0, drop =True )
temp['%D'] = temp.groupby('code')['%k'].rolling(smoothing_window).mean().reset_index(level=0, drop = True)

```

The original loop takes 10 secs, vectorized operation takes 0.6 secs
Same goes for creating insample and outsample data

```
x = data_f1.groupby('code').apply(lambda x: x[:'20220630150000000']).reset_index(level=0, drop=True)
y = data_f1.groupby('code').apply(lambda x: x['20220701100000000':]).reset_index(level=0, drop=True)
```
