//@version=5
strategy("Pivot Reversal Strategy", overlay=true, default_qty_type=strategy.percent_of_equity, default_qty_value=10)

// Inputs for pivot period, stop loss, and target levels
pivotLeft = input.int(3, title="Pivot Left Bars", minval=1)
pivotRight = input.int(3, title="Pivot Right Bars", minval=1)
stopLossPercent = input.float(1.0, title="Stop Loss (%)", minval=0.1) / 100
takeProfitPercent = input.float(2.0, title="Take Profit (%)", minval=0.1) / 100

// Calculate pivot highs and lows
pivotHigh = ta.pivothigh(high, pivotLeft, pivotRight)
pivotLow = ta.pivotlow(low, pivotLeft, pivotRight)

// Initialize stop-loss and take-profit levels
var float stopLoss = na
var float takeProfit = na

// Conditions for long entry
if (pivotLow)
    stopLoss := low[pivotRight] * (1 - stopLossPercent)
    takeProfit := close * (1 + takeProfitPercent)
    strategy.entry("Long", strategy.long, stop=stopLoss, limit=takeProfit)

// Conditions for short entry
if (pivotHigh)
    stopLoss := high[pivotRight] * (1 + stopLossPercent)
    takeProfit := close * (1 - takeProfitPercent)
    strategy.entry("Short", strategy.short, stop=stopLoss, limit=takeProfit)

// Plot pivot points
plotshape(series=pivotHigh, location=location.abovebar, color=color.red, style=shape.labelup, text="Sell", offset=-pivotRight)
plotshape(series=pivotLow, location=location.belowbar, color=color.green, style=shape.labeldown, text="Buy", offset=-pivotRight)

// Plot stop-loss and take-profit levels
plot(stopLoss, color=color.red, title="Stop Loss Level")
plot(takeProfit, color=color.green, title="Take Profit Level")# Super-duper-stock-market
