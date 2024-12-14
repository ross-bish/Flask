# Flask Project: Data Visualizations

## Basic Flask Application

### app.py (Basic Version)
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")  # Basic template file

if __name__ == "__main__":
    app.run(debug=True)
```

### Folder Structure (Basic Version)
```
project/
├── app.py                      # Flask application
├── templates/
│   └── index.html              # HTML file to display visualizations
├── static/
│   ├── population_chart.png    # Static bar chart image
│   └── growth_rate_chart.png   # Static line chart image
└── population_data.csv         # Data file
```

### Data Visualization Code (Basic Version)
```python
import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
df = pd.read_csv("population_data.csv")

# Create a bar chart for Population
plt.figure(figsize=(10, 6))
plt.bar(df["Year"], df["Population"], color="skyblue")
plt.title("Population Over the Years")
plt.xlabel("Year")
plt.ylabel("Population")
plt.savefig("static/population_chart.png")  # Save chart in static folder

# Create a line chart for Growth Rate
plt.figure(figsize=(10, 6))
plt.plot(df["Year"], df["Growth Rate"], marker="o", color="orange")
plt.title("Growth Rate Over the Years")
plt.xlabel("Year")
plt.ylabel("Growth Rate (%)")
plt.savefig("static/growth_rate_chart.png")  # Save chart in static folder
```

### index.html (Basic Version)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basic Data Visualizations</title>
</head>
<body>
    <h1>Basic Population Data Visualization</h1>

    <h2>Population Over the Years</h2>
    <img src="/static/population_chart.png" alt="Population Chart">

    <h2>Growth Rate Over the Years</h2>
    <img src="/static/growth_rate_chart.png" alt="Growth Rate Chart">
</body>
</html>
```

---

## Interactive Flask Application (with Plotly)

### app.py (Interactive Version)
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("interactive_index.html")

@app.route("/population_chart.html")
def population_chart():
    return render_template("population_chart.html")

@app.route("/growth_rate_chart.html")
def growth_rate_chart():
    return render_template("growth_rate_chart.html")

if __name__ == "__main__":
    app.run(debug=True)
```

### Folder Structure (Interactive Version)
```
project/
├── app.py                      # Flask application
├── templates/
│   ├── interactive_index.html  # Main template file
│   ├── population_chart.html   # Interactive bar chart HTML
│   └── growth_rate_chart.html  # Interactive line chart HTML
└── population_data.csv         # Data file
```

### Data Visualization Code (Interactive Version)
```python
import pandas as pd
import plotly.express as px

# Load the dataset
df = pd.read_csv("population_data.csv")

# Create an interactive bar chart for Population
population_chart = px.bar(
    df,
    x="Year",
    y="Population",
    title="Population Over the Years",
    labels={"Population": "Population", "Year": "Year"},
    template="plotly_white"
)
population_chart.update_traces(hovertemplate="Year: %{x}<br>Population: %{y:,}")
population_chart.write_html("templates/population_chart.html")  # Save as HTML

# Create an interactive line chart for Growth Rate
growth_rate_chart = px.line(
    df,
    x="Year",
    y="Growth Rate",
    title="Growth Rate Over the Years",
    labels={"Growth Rate": "Growth Rate (%)", "Year": "Year"},
    markers=True,
    template="plotly_white"
)
growth_rate_chart.update_traces(hovertemplate="Year: %{x}<br>Growth Rate: %{y:.2f}%")
growth_rate_chart.write_html("templates/growth_rate_chart.html")  # Save as HTML
```

### interactive_index.html (Interactive Version)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Data Visualizations</title>
</head>
<body>
    <h1>Interactive Population Data Visualization</h1>

    <h2>Population Over the Years</h2>
    <iframe src="population_chart.html" width="100%" height="500" style="border:none;"></iframe>

    <h2>Growth Rate Over the Years</h2>
    <iframe src="growth_rate_chart.html" width="100%" height="500" style="border:none;"></iframe>
</body>
</html>
```