# Interactive Financial Dashboard - Stock Analysis Platform

A sophisticated web-based financial dashboard built with Bokeh for interactive stock market analysis and comparison. Features real-time data visualization, technical indicators, and synchronized dual-stock comparison with advanced charting capabilities.

## 📈 Features

- **Dual Stock Comparison**: Side-by-side analysis of two stocks with synchronized charts
- **Interactive Candlestick Charts**: Professional-grade OHLC candlestick visualization
- **Technical Indicators**: Multiple technical analysis tools including SMAs and linear regression
- **Real-time Data**: Live stock data integration via Yahoo Finance (yfinance)
- **Synchronized Navigation**: Linked chart navigation for comparative analysis
- **Date Range Selection**: Flexible date picker for historical data analysis
- **Responsive Web Interface**: Built with Bokeh server for dynamic interactions

## 🛠 Tech Stack

- **Visualization**: Bokeh (Interactive web plotting library)
- **Data Source**: yfinance (Yahoo Finance API wrapper)
- **Data Processing**: NumPy for numerical computations
- **Web Framework**: Bokeh Server for real-time web applications
- **Mathematical Analysis**: NumPy polynomial fitting for regression analysis

## 📦 Installation

1. Clone the repository:
```bash
git clone https://github.com/machapraveen/Financial-Dashboard.git
cd "Financial Dashboard"
```

2. Install required dependencies:
```bash
pip install bokeh yfinance numpy
```

## 🚀 Usage

1. Start the Bokeh server:
```bash
bokeh serve main.py --show
```

2. Open your browser to `http://localhost:5006/main`

3. Use the interface to:
   - Enter two stock symbols for comparison
   - Select date range using date pickers
   - Choose technical indicators to display
   - Click "Load Data" to generate interactive charts

## 📊 Technical Indicators

### Simple Moving Averages (SMA)
- **30-Day SMA**: Short-term trend analysis
- **100-Day SMA**: Medium-term trend identification

### Linear Regression Line
- Trend direction and strength analysis
- Statistical trend fitting using NumPy polynomial regression

## 🎯 Core Functions

### Data Loading
```python
def load_data(ticker1, ticker2, start, end):
    df1 = yf.download(ticker1, start, end)
    df2 = yf.download(ticker2, start, end)
    return df1, df2
```

### Chart Generation
```python
def update_plot(data, indicators, sync_axis=None):
    df = data
    gain = df.Close > df.Open
    loss = df.Open > df.Close
    
    # Create candlestick chart
    p = figure(x_axis_type="datetime", tools="pan,wheel_zoom,box_zoom,reset,save")
    p.segment(df.index, df.High, df.index, df.Low, color="black")
    p.vbar(df.index[gain], width, df.Open[gain], df.Close[gain], fill_color="#00ff00")
    p.vbar(df.index[loss], width, df.Open[loss], df.Close[loss], fill_color="#ff0000")
    
    return p
```

### Technical Indicator Implementation
```python
# 30-Day Simple Moving Average
if indicator == "30 Day SMA":
    df['SMA30'] = df['Close'].rolling(30).mean()
    p.line(df.index, df.SMA30, color="purple", legend_label="30 Day SMA")

# Linear Regression Analysis
elif indicator == "Linear Regression Line":
    par = np.polyfit(range(len(df.index.values)), df.Close.values, 1, full=True)
    slope, intercept = par[0][0], par[0][1]
    y_predicted = [slope * i + intercept for i in range(len(df.index.values))]
    p.segment(df.index[0], y_predicted[0], df.index[-1], y_predicted[-1], 
              legend_label="Linear Regression", color="red")
```

## 🎨 Interactive Features

### Chart Synchronization
- X-axis synchronization between comparison charts
- Unified zoom and pan controls
- Coordinated time series navigation

### Dynamic Controls
```python
# Interactive widgets
stock1_text = TextInput(title="Main Stock")
stock2_text = TextInput(title="Comparison Stock")
date_picker_from = DatePicker(title='Start Date', value="2020-01-01")
date_picker_to = DatePicker(title='End Date', value="2020-02-01")
indicator_choice = MultiChoice(options=["100 Day SMA", "30 Day SMA", "Linear Regression Line"])
```

### Real-time Updates
```python
def on_button_click(main_stock, comparison_stock, start, end, indicators):
    source1, source2 = load_data(main_stock, comparison_stock, start, end)
    p = update_plot(source1, indicators)
    p2 = update_plot(source2, indicators, sync_axis=p.x_range)
    curdoc().clear()
    curdoc().add_root(layout)
    curdoc().add_root(row(p, p2))
```

## 📋 User Interface

### Input Controls
- **Main Stock**: Primary stock symbol input
- **Comparison Stock**: Secondary stock for comparison
- **Start Date**: Historical data start point
- **End Date**: Historical data end point
- **Indicators**: Multi-select technical indicator options

### Chart Features
- **Candlestick Visualization**: Green/red candles for price action
- **High/Low Wicks**: Complete OHLC representation
- **Interactive Tools**: Pan, zoom, box zoom, reset, save
- **Legend Controls**: Click to hide/show indicators
- **Responsive Layout**: Automatic chart resizing

## 🔧 Chart Configuration

### Styling Options
```python
p.xaxis.major_label_orientation = math.pi / 4  # 45-degree angle
p.grid.grid_line_alpha = 0.3                   # Subtle grid lines
p.legend.location = "top_left"                 # Legend positioning
p.legend.click_policy = "hide"                 # Interactive legend
```

### Color Schemes
- **Bullish Candles**: Green (#00ff00)
- **Bearish Candles**: Red (#ff0000)
- **30-Day SMA**: Purple
- **100-Day SMA**: Blue
- **Linear Regression**: Red

## 📈 Example Analysis Workflows

### Trend Analysis
1. Load two similar stocks (e.g., AAPL vs MSFT)
2. Enable 30-day and 100-day SMAs
3. Add linear regression line
4. Compare trend directions and crossovers

### Volatility Comparison
1. Select stocks from different sectors
2. Use synchronized charts for direct comparison
3. Analyze candlestick patterns
4. Compare high-low ranges

## 🔮 Future Enhancements

- Additional technical indicators (RSI, MACD, Bollinger Bands)
- Volume analysis charts
- Multiple timeframe analysis
- Stock screening capabilities
- Portfolio tracking features
- Export functionality for charts and data

## 👨‍💻 Author

**Macha Praveen**
- GitHub: [@machapraveen](https://github.com/machapraveen)

## 📄 License

This project is open source and available under the MIT License.
