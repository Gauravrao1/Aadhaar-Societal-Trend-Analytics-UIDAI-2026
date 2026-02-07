# Aadhaar Societal Trend Analytics - User Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Quick Start](#quick-start)
4. [Core Modules](#core-modules)
5. [API Reference](#api-reference)
6. [Examples](#examples)
7. [Best Practices](#best-practices)

## Introduction

This system provides comprehensive analytics for Aadhaar enrolment data to support data-driven governance decisions for UIDAI. It enables:
- Detection of seasonal patterns in enrolment demand
- Monitoring of district-level capacity and pressure
- Prediction of future enrolment demand
- Data visualization and reporting

## Installation

### System Requirements
- Python 3.8 or higher
- 4GB RAM minimum
- 1GB free disk space

### Dependencies Installation
```bash
pip install -r requirements.txt
```

## Quick Start

### Running the Main Analysis
```bash
python src/main.py
```

This will:
1. Generate sample data for demonstration
2. Perform all three types of analysis
3. Create visualizations in the `outputs/` directory
4. Display a comprehensive report

### Using Your Own Data

Create a CSV file with the following format:
```csv
district,state,date,enrolments
Mumbai,Maharashtra,2024-01-01,150
Delhi,Delhi,2024-01-01,200
```

Then load and analyze:
```python
from src.main import AadhaarAnalytics
from src.utils.data_utils import load_data_from_csv

# Load your data
data = load_data_from_csv('path/to/your/data.csv')

# Initialize analytics
analytics = AadhaarAnalytics(data)

# Generate report
report = analytics.generate_comprehensive_report()
```

## Core Modules

### 1. Seasonal Trend Detection

Identifies patterns in enrolment data over time.

**Key Features:**
- Monthly and quarterly peak identification
- Seasonality strength measurement
- Anomaly detection
- Time series decomposition

**Usage:**
```python
from src.analysis.seasonal_trends import SeasonalTrendDetector

detector = SeasonalTrendDetector(data)
summary = detector.get_seasonal_summary()

print(f"Peak Months: {summary['peak_months']}")
print(f"Seasonality: {summary['seasonality_strength']}")
```

### 2. District Pressure Analysis

Monitors capacity and pressure at district level.

**Key Features:**
- District-level metrics calculation
- High-pressure district identification
- Capacity requirement estimation
- Surge detection

**Usage:**
```python
from src.analysis.district_pressure import DistrictPressureAnalyzer

analyzer = DistrictPressureAnalyzer(data)
metrics = analyzer.calculate_district_metrics()
high_pressure = analyzer.identify_high_pressure_districts()
```

### 3. Predictive Demand Modeling

Forecasts future enrolment demand.

**Key Features:**
- Multiple prediction models
- Demand indicator calculation
- Peak period forecasting
- Growth rate analysis

**Usage:**
```python
from src.analysis.predictive_demand import DemandPredictor

predictor = DemandPredictor(data)
predictor.train_linear_model()
predictions = predictor.predict_next_period(days=30)
indicators = predictor.calculate_demand_indicators()
```

## API Reference

### AadhaarAnalytics Class

Main interface for all analytics operations.

#### Methods

**`__init__(data: pd.DataFrame)`**
- Initialize analytics with enrolment data
- Parameters: 
  - `data`: DataFrame with columns: district, state, date, enrolments

**`analyze_seasonal_trends(district: Optional[str] = None) -> Dict`**
- Analyze seasonal patterns
- Parameters:
  - `district`: Optional district name for district-specific analysis
- Returns: Dictionary with seasonal insights

**`analyze_district_pressure() -> Dict`**
- Analyze district-level pressure and capacity
- Returns: Dictionary with pressure metrics and high-pressure districts

**`predict_demand(district: Optional[str] = None, days: int = 30) -> Dict`**
- Predict future enrolment demand
- Parameters:
  - `district`: Optional district name
  - `days`: Number of days to predict ahead
- Returns: Dictionary with predictions and indicators

**`generate_comprehensive_report() -> Dict`**
- Generate complete analytics report
- Returns: Dictionary with all analysis results

**`create_visualizations(output_dir: str = 'outputs')`**
- Create and save all visualizations
- Parameters:
  - `output_dir`: Directory to save visualizations

### SeasonalTrendDetector Class

#### Key Methods

**`identify_peak_months() -> Dict[int, float]`**
- Returns: Dictionary mapping month number to average enrolments

**`identify_peak_quarters() -> Dict[int, float]`**
- Returns: Dictionary mapping quarter number to average enrolments

**`calculate_seasonality_strength() -> float`**
- Returns: Seasonality strength score (0-1)

**`detect_anomalous_periods(threshold: float = 2.0) -> List[Dict]`**
- Detect periods with anomalous patterns
- Parameters:
  - `threshold`: Number of standard deviations for anomaly detection
- Returns: List of anomalous periods

**`detect_seasonal_pattern(period: int = 12) -> Dict`**
- Perform time series decomposition
- Parameters:
  - `period`: Seasonality period (default: 12 for monthly)
- Returns: Dictionary with trend, seasonal, and residual components

### DistrictPressureAnalyzer Class

#### Key Methods

**`calculate_district_metrics(window_days: int = 30) -> pd.DataFrame`**
- Calculate key metrics for each district
- Parameters:
  - `window_days`: Rolling window for metrics calculation
- Returns: DataFrame with district-level metrics

**`identify_high_pressure_districts(threshold_percentile: float = 75) -> List[Dict]`**
- Identify districts with high pressure
- Parameters:
  - `threshold_percentile`: Percentile threshold
- Returns: List of high-pressure districts

**`calculate_capacity_utilization(capacity_per_centre: int = 100) -> pd.DataFrame`**
- Calculate capacity utilization
- Parameters:
  - `capacity_per_centre`: Daily capacity per enrolment centre
- Returns: DataFrame with capacity metrics

**`detect_surges(surge_threshold: float = 2.0) -> List[Dict]`**
- Detect sudden surges in enrolments
- Parameters:
  - `surge_threshold`: Multiplier threshold
- Returns: List of surge events

### DemandPredictor Class

#### Key Methods

**`train_linear_model() -> Dict`**
- Train a linear regression prediction model
- Returns: Dictionary with model performance metrics

**`predict_next_period(days: int = 30, model_type: str = 'linear') -> pd.DataFrame`**
- Predict enrolments for next period
- Parameters:
  - `days`: Number of days to predict
  - `model_type`: 'linear', 'ma', or 'exponential'
- Returns: DataFrame with predictions

**`calculate_demand_indicators() -> Dict`**
- Calculate key demand indicators
- Returns: Dictionary with growth rate, volatility, trends

**`identify_peak_demand_periods(future_days: int = 90) -> List[Dict]`**
- Identify predicted peak periods
- Parameters:
  - `future_days`: Days to predict ahead
- Returns: List of peak demand periods

## Examples

### Example 1: Analyze Specific District

```python
from src.main import AadhaarAnalytics
from src.utils.data_utils import load_data_from_csv

data = load_data_from_csv('data.csv')
analytics = AadhaarAnalytics(data)

# Analyze Mumbai specifically
mumbai_trends = analytics.analyze_seasonal_trends(district='Mumbai')
mumbai_predictions = analytics.predict_demand(district='Mumbai', days=60)

print(f"Mumbai peak months: {mumbai_trends['peak_months']}")
print(f"Mumbai 60-day forecast: {mumbai_predictions['predictions']}")
```

### Example 2: Monitor High-Pressure Districts

```python
from src.analysis.district_pressure import DistrictPressureAnalyzer

analyzer = DistrictPressureAnalyzer(data)

# Get high pressure districts
high_pressure = analyzer.identify_high_pressure_districts(threshold_percentile=80)

for district in high_pressure:
    print(f"{district['district']}: Pressure Score = {district['pressure_score']:.2f}")
    print(f"  Trend: {district['trend_percentage']:+.1f}%")
```

### Example 3: Forecast Peak Periods

```python
from src.analysis.predictive_demand import DemandPredictor

predictor = DemandPredictor(aggregated_data)
predictor.train_linear_model()

# Identify next 90 days peak periods
peaks = predictor.identify_peak_demand_periods(future_days=90)

print("Expected peak periods:")
for peak in peaks[:10]:
    print(f"  {peak['date']}: {peak['predicted_enrolments']:.0f} enrolments")
```

### Example 4: Create Custom Visualizations

```python
from src.visualization.plots import EnrolmentVisualizer

visualizer = EnrolmentVisualizer()

# Time series plot
visualizer.plot_time_series(
    data, 
    title="My Custom Title",
    save_path='my_plot.png'
)

# District comparison
visualizer.plot_district_comparison(
    metrics,
    metric='total_enrolments',
    top_n=15,
    save_path='top_districts.png'
)
```

## Best Practices

### Data Quality
1. Ensure date column is in consistent format (YYYY-MM-DD)
2. Check for missing values and handle appropriately
3. Verify district and state names are consistent
4. Remove or flag obviously erroneous data points

### Analysis
1. Use at least 2 years of historical data for seasonal analysis
2. Consider weekly patterns when analyzing district pressure
3. Validate predictions against known events (holidays, campaigns)
4. Run analysis regularly to track trends over time

### Performance
1. Aggregate data by day/week for faster processing of large datasets
2. Filter to specific districts when analyzing individual areas
3. Use appropriate time windows for rolling calculations
4. Cache model training results for repeated predictions

### Interpretation
1. Consider external factors (policy changes, campaigns) when interpreting trends
2. Cross-validate predictions with multiple models
3. Use confidence intervals when making capacity decisions
4. Monitor both absolute numbers and percentage changes

### Visualization
1. Choose appropriate time scales for your audience
2. Highlight actionable insights in reports
3. Use interactive dashboards for exploratory analysis
4. Save high-resolution plots for formal presentations

## Troubleshooting

### Common Issues

**Issue: "Invalid frequency: M"**
- Solution: Update pandas to version 2.0+, or code will use 'ME' automatically

**Issue: "Insufficient data for seasonal decomposition"**
- Solution: Ensure you have at least 2 complete seasonal cycles (24 months for monthly data)

**Issue: Model training fails**
- Solution: Check that you have enough non-null data points (minimum 10)

**Issue: High memory usage**
- Solution: Aggregate data to weekly or monthly level before analysis

## Support

For issues, questions, or contributions, please open an issue on the GitHub repository.
