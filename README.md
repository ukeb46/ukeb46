# TradingView Buy/Sell Signal Indicator

## Overview
This is a Pine Script v6 indicator that generates automated buy and sell signals for TradingView charts. The indicator uses a custom signal library to identify potential trading opportunities based on price action analysis.

**Author:** yashgode9 (YASH NANDKUMAR GODE)

## Features

### Signal Generation
- **Automated Buy/Sell Detection**: Uses zigzag-based pattern recognition to identify market reversals
- **Visual Indicators**: Displays triangular markers on the chart
  - Green triangle pointing up (▲) for BUY signals (below bar)
  - Red triangle pointing down (▼) for SELL signals (above bar)
- **Dynamic Labels**: Shows "Buy-point" and "Sell-point" labels with customizable appearance
- **Repainting Mode**: Currently set to `true` for real-time signal updates

### Customizable Parameters

#### Signal Engine Configuration
- **DEPTH_ENGINE** (default: 30): Controls the depth of price analysis
  - Minimum: 1
  - Higher values = less sensitive, fewer signals
  - Lower values = more sensitive, more signals

- **DEVIATION_ENGINE** (default: 5): Sets the minimum price deviation threshold
  - Minimum: 1
  - Controls signal filtering sensitivity

- **BACKSTEP_ENGINE** (default: 5): Prevents premature signal generation
  - Minimum: 2
  - Filters out noise in volatile markets

#### Visual Customization
- **Labels Transparency** (default: 0): Controls label opacity (0-100)
- **Buy Color** (default: #03ff85 - bright green): Customizable buy signal color
- **Sell Color** (default: #fc0808 - bright red): Customizable sell signal color
- **Label Size** (default: 3 - normal): Choose from 5 sizes
  1. Tiny
  2. Small
  3. Normal
  4. Large
  5. Huge

### Alert System
The indicator includes a comprehensive alert system:

1. **Built-in Alerts**: Triggered on every signal change
   - "Buy signal generated!!!" for buy signals
   - "Sell signal generated!!!" for sell signals

2. **Conditional Alerts**: User-controllable alerts
   - **Enable Buy Alerts** (default: true)
   - **Enable Sell Alerts** (default: true)
   - Includes ticker symbol and current price in alert message

### Technical Details

#### Signal Logic
The indicator uses the `signalLib_yashgode9/2` library which returns:
- `direction`: Current market direction (negative = buy, positive = sell)
- `zee1`: Primary zigzag point data
- `zee2`: Secondary zigzag point data

**Buy Signal Conditions:**
- Direction changes (`ta.change(direction) != 0`)
- AND direction is negative (`direction < 0`)

**Sell Signal Conditions:**
- Direction changes (`ta.change(direction) != 0`)
- AND direction is positive (`direction > 0`)

#### Resource Limits
- Maximum Labels: 200
- Maximum Lines: 50

## Installation

1. Open TradingView
2. Go to Pine Editor
3. Create a new indicator
4. Copy and paste the code from `buysellsignal-yashgode9.pine`
5. Click "Add to Chart"

## Usage

### Basic Setup
1. Add the indicator to your chart
2. The default settings work well for most timeframes
3. Adjust parameters based on your trading style:
   - **1-Second Charts (Scalping)**: DEPTH_ENGINE (5-10), DEVIATION_ENGINE (2-3), BACKSTEP_ENGINE (2-3)
   - **Day Trading**: Lower DEPTH_ENGINE (15-25) for more signals
   - **Swing Trading**: Higher DEPTH_ENGINE (35-50) for fewer, stronger signals

### 1-Second Chart Optimization (NEW!)
The indicator now includes special optimizations for ultra-fast 1-second charts:

#### Automatic Parameter Detection
- Automatically detects when running on 1s, 5s, or sub-minute timeframes
- Provides optimized default values for each timeframe type
- Suggested settings appear in parameter tooltips

#### Noise Filtering (Highly Recommended for 1s Charts)
- **ATR-Based Filter**: Eliminates false signals from market noise
  - `ATR Çarpanı`: 0.2-0.5 for 1s charts (default: 0.3)
  - Lower values = more signals, Higher values = fewer but higher quality signals

- **Volume Filter** (Optional): Confirms signals with volume strength
  - Helps filter out weak movements
  - Can be enabled/disabled based on market conditions

- **Consecutive Signal Prevention**: Prevents rapid-fire signals
  - Default: 2 bars between same-direction signals
  - Reduces overtrading on 1s charts

#### Debug Features
- **Debug Info Panel**: Shows real-time indicator status
  - Current timeframe
  - Active parameter values
  - ATR and minimum movement thresholds
  - Filter status (active/inactive)

- **Filtered Signal Visualization**: See which signals were blocked by filters
  - Gray X marks show filtered-out signals
  - Helps you tune filter parameters

#### Performance Tips for 1s Charts
1. **Start Conservative**: Use default 1s settings (Depth=8, Deviation=2, Backstep=2)
2. **Enable Noise Filter**: Critical for 1s timeframes to avoid false signals
3. **Monitor ATR**: Adjust ATR multiplier based on market volatility
4. **Use Debug Mode**: Enable temporarily to understand signal behavior
5. **Disable Volume Filter**: Unless trading high-liquidity assets
6. **Test in Replay Mode**: Backtest your settings before live trading

### Setting Up Alerts
1. Right-click on the indicator name in the chart
2. Select "Add Alert on buysellsignal-yashgode9"
3. Choose:
   - "Buy Alert" for buy signal notifications
   - "Sell Alert" for sell signal notifications
4. Configure your notification preferences (email, SMS, webhook, etc.)

### Best Practices
- **Confirm Signals**: Don't trade solely based on automated signals
- **Use Stop Losses**: Always implement proper risk management
- **Backtest First**: Test the indicator on historical data
- **Adjust Parameters**: Optimize settings for your specific market and timeframe
- **Combine with Other Indicators**: Use alongside volume, RSI, MACD, etc.

## Signal Interpretation

### Buy Signals (Green Triangle ▲)
- Indicates potential bullish reversal
- Direction has changed to negative
- Price may be at a local bottom
- Consider entering long positions

### Sell Signals (Red Triangle ▼)
- Indicates potential bearish reversal
- Direction has changed to positive
- Price may be at a local top
- Consider taking profits or entering short positions

## Configuration Groups

The indicator settings are organized into logical groups:

1. **signalLib Config**: Core signal generation parameters (auto-optimized for 1s charts)
2. **Filters**: Noise reduction and signal quality controls (NEW!)
   - Noise filter with ATR-based detection
   - Volume confirmation filter
   - Consecutive signal prevention
3. **Alerts**: Enable/disable alert types
4. **Labels**: Visual appearance of labels
5. **Colors**: Customizable color scheme
6. **Debug**: Performance monitoring and signal analysis tools (NEW!)

## Important Notes

### Repainting
- The indicator is currently set to `repaint = true`
- This means signals may change on the current bar until it closes
- For non-repainting signals, this setting would need to be modified

### Dependencies
- Requires `yashgode9/signalLib_yashgode9/2` library
- Must be properly imported for the indicator to function

## Troubleshooting

### No Signals Appearing
- Increase DEPTH_ENGINE value
- Check if the signalLib library is properly imported
- Verify the indicator is applied to the correct price data

### Too Many Signals
- Increase DEPTH_ENGINE value (try 40-50)
- Increase DEVIATION_ENGINE value
- Increase BACKSTEP_ENGINE value
- **For 1s charts**: Enable Noise Filter and increase ATR Multiplier (0.4-0.6)
- Enable Volume Filter for additional confirmation

### Too Few Signals (1s Charts)
- Decrease DEPTH_ENGINE (try 5-7)
- Decrease ATR Multiplier (0.2-0.25)
- Disable Volume Filter
- Check Debug Info to verify filter status

### False Signals on 1s Charts
- Increase ATR Multiplier (0.4-0.6)
- Enable Volume Filter
- Increase Consecutive Signal Bars (3-5)
- Use higher DEVIATION_ENGINE (3-4)

### Alerts Not Working
- Verify "Enable Buy Alerts" or "Enable Sell Alerts" is checked
- Ensure you've created the alert in TradingView
- Check alert frequency settings (set to once_per_bar_close)

## Code Structure

```
buysellsignal-yashgode9.pine
├── Import signalLib library
├── Input parameters configuration
├── Signal calculation using signalLib
├── Label and line drawing logic
├── Buy/Sell signal detection
├── Alert generation
└── Visual indicators (plotshape)
```

## Version Information
- **Pine Script Version**: v6
- **Library Version**: signalLib_yashgode9/2
- **Max Labels**: 200
- **Max Lines**: 50

## License
Created by yashgode9 (YASH NANDKUMAR GODE)

## Disclaimer
This indicator is for educational purposes only. Trading involves substantial risk of loss. Past performance is not indicative of future results. Always do your own research and consult with financial advisors before making trading decisions.
