+++
title = "Presentation"
authorTwitter = "" #do not include @
cover = ""
showFullContent = true
+++


<img style="margin: 0 auto" src="https://raw.githubusercontent.com/anfederico/Clairvoyant/master/media/Logo.png" alt="drawing" width="200"/>

## Presentation
```python
pip install clairvoyant
```

## Latest Development Changes
```bash
git clone https://github.com/anfederico/Clairvoyant
```

## Backtesting Signal Accuracy
During the testing period, the model signals to buy or sell based on its prediction for price
movement the following day. 
alone, you can rapidly build and test more reliable models.

```python
from clairvoyant.engine import Backtest
import pandas as pd

features  = ["EMA", "SSO"]   # Financial indicators of choice
trainStart = 0               # Start of training period
trainEnd   = 700             # End of training period
testStart  = 701             # Start of testing period
testEnd    = 1000            # End of testing period
buyThreshold  = 0.65         # Confidence threshold for predicting buy (default = 0.65) 
sellThreshold = 0.65         # Confidence threshold for predicting sell (default = 0.65)
continuedTraining = False    # Continue training during testing period? (default = false)

# Initialize backtester
backtest = Backtest(features, trainStart, trainEnd, testStart, testEnd, buyThreshold, sellThreshold, continuedTraining)

# A little bit of pre-processing
data = pd.read_csv("SBUX.csv", date_parser=['date'])
data = data.round(3)

# Start backtesting and optionally modify SVC parameters
# Available paramaters can be found at: http://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html
backtest.start(data, kernel='rbf', C=1, gamma=10)
backtest.conditions()
backtest.statistics()
backtest.visualize('SBUX')
```
