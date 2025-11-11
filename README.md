# IBM-Python-Project-Stock-Analysis
# 📊 Stock Data Analysis: Tesla & GameStop
📝 Project Description
This project analyzes historical stock data and revenue information for Tesla (TSLA) and GameStop (GME) using Python. The analysis includes data extraction using yfinance API, web scraping for revenue data, and interactive visualizations.

🛠️ Technologies & Libraries Used

Python 3.x
yfinance - Stock market data extraction
BeautifulSoup4 - Web scraping
Requests - HTTP requests
Pandas - Data manipulation and analysis
Plotly - Interactive data visualization


📚 Installation
python!pip install yfinance
!pip install beautifulsoup4
!pip install plotly

📋 Project Questions & Solutions

❓ Question 1: Use yfinance to Extract Tesla Stock Data
Description: Extract Tesla stock data using the yfinance library and display the first 5 rows.
Code:
pythonimport yfinance as yf
import pandas as pd

# Create ticker object for Tesla
tesla = yf.Ticker("TSLA")

# Extract historical data for maximum period
tesla_data = tesla.history(period="max")

# Reset index
tesla_data.reset_index(inplace=True)

# Display first 5 rows
print(tesla_data.head())
Output:
Show Image

❓ Question 2: Use Webscraping to Extract Tesla Revenue Data
Description: Extract Tesla revenue data using web scraping and display the last 5 rows.
Code:
pythonimport requests
from bs4 import BeautifulSoup
import pandas as pd

# URL for Tesla revenue data
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"

# Get HTML content
html_data = requests.get(url).text

# Parse HTML
soup = BeautifulSoup(html_data, 'html.parser')

# Create empty DataFrame
tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])

# Find all tables
tables = soup.find_all('table')

# Extract data from table
for row in tables[0].tbody.find_all('tr'):
    col = row.find_all('td')
    if len(col) == 2:
        date = col[0].text.strip()
        revenue = col[1].text.strip()
        tesla_revenue = pd.concat([tesla_revenue, pd.DataFrame({
            "Date": [date], 
            "Revenue": [revenue]
        })], ignore_index=True)

# Clean data - remove commas and dollar signs
tesla_revenue["Revenue"] = tesla_revenue["Revenue"].str.replace(',', '').str.replace('$', '')

# Remove null values
tesla_revenue.dropna(inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]

# Display last 5 rows
print(tesla_revenue.tail())
Output:
Show Image

❓ Question 3: Use yfinance to Extract GameStop Stock Data
Description: Extract GameStop stock data using the yfinance library and display the first 5 rows.
Code:
pythonimport yfinance as yf
import pandas as pd

# Create ticker object for GameStop
gamestop = yf.Ticker("GME")

# Extract historical data for maximum period
gme_data = gamestop.history(period="max")

# Reset index
gme_data.reset_index(inplace=True)

# Display first 5 rows
print(gme_data.head())
Output:
Show Image

❓ Question 4: Use Webscraping to Extract GameStop Revenue Data
Description: Extract GameStop revenue data using web scraping and display the last 5 rows.
Code:
pythonimport requests
from bs4 import BeautifulSoup
import pandas as pd

# URL for GameStop revenue data
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"

# Get HTML content
html_data = requests.get(url).text

# Parse HTML
soup = BeautifulSoup(html_data, 'html.parser')

# Create empty DataFrame
gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])

# Find all tables
tables = soup.find_all('table')

# Extract data from table
for row in tables[0].tbody.find_all('tr'):
    col = row.find_all('td')
    if len(col) == 2:
        date = col[0].text.strip()
        revenue = col[1].text.strip()
        gme_revenue = pd.concat([gme_revenue, pd.DataFrame({
            "Date": [date], 
            "Revenue": [revenue]
        })], ignore_index=True)

# Clean data - remove commas and dollar signs
gme_revenue["Revenue"] = gme_revenue["Revenue"].str.replace(',', '').str.replace('$', '')

# Remove null values
gme_revenue.dropna(inplace=True)
gme_revenue = gme_revenue[gme_revenue['Revenue'] != ""]

# Display last 5 rows
print(gme_revenue.tail())
Output:
Show Image

❓ Question 5: Plot Tesla Stock Graph
Description: Create an interactive graph showing Tesla stock price and revenue over time using the make_graph function.
Code:
pythonimport plotly.graph_objects as go
from plotly.subplots import make_subplots
import pandas as pd

def make_graph(stock_data, revenue_data, stock):
    """
    Create interactive plot with stock price and revenue
    """
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, 
                        subplot_titles=(stock + " Share Price", stock + " Revenue"), 
                        vertical_spacing = .3)
    
    stock_data_specific = stock_data[stock_data.Date <= '2021-06-14']
    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']
    
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data_specific.Date, infer_datetime_format=True), 
                             y=stock_data_specific.Close.astype("float"), 
                             name="Share Price"), row=1, col=1)
    
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data_specific.Date, infer_datetime_format=True), 
                             y=revenue_data_specific.Revenue.astype("float"), 
                             name="Revenue"), row=2, col=1)
    
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False, height=900, title=stock, xaxis_rangeslider_visible=True)
    
    fig.show()

# Plot Tesla graph
make_graph(tesla_data, tesla_revenue, 'Tesla')
Output:
Show Image
The graph shows Tesla's stock price trend in the upper panel and revenue growth in the lower panel from 2010 to 2021.

❓ Question 6: Plot GameStop Stock Graph
Description: Create an interactive graph showing GameStop stock price and revenue over time using the make_graph function.
Code:
python# Plot GameStop graph
make_graph(gme_data, gme_revenue, 'GameStop')
Output:
Show Image
The graph displays GameStop's stock price volatility in the upper panel and revenue trends in the lower panel, notably showing the 2021 short squeeze event.

📊 Key Findings
Tesla (TSLA)

Shows strong upward trend in stock price from 2019-2021
Revenue demonstrates consistent growth year over year
Stock experienced significant appreciation during the electric vehicle boom

GameStop (GME)

Notable volatility in stock price, especially during 2021
The famous "short squeeze" event is clearly visible in the data
Revenue trends show the challenges facing traditional retail gaming


🚀 How to Run This Project

Clone the repository:

bashgit clone https://github.com/your-username/Stock-Analysis-Project.git

Open in Google Colab or Jupyter Notebook
Install required libraries:

python!pip install yfinance beautifulsoup4 plotly

Run all cells sequentially


📁 Project Structure
IBM-Python-Project-Stock-Analysis/
│
├── README.md                          # Project documentation
├── Stock_Analysis.ipynb               # Main Jupyter notebook
│
└── screenshots/                       # Output screenshots
    ├── Q_1.png
    ├── Q_2.png
    ├── Q_3.png
    ├── Q_4.png
    ├── Q_5.png
    ├── Q_5_Pic.png
    ├── Q_6.png
    └── Q_6_Pic.png

📸 Screenshots
All screenshots are located in the screenshots/ folder and are embedded in the questions above.

🎓 Learning Outcomes
Through this project, I learned:

✅ How to extract financial data using APIs (yfinance)
✅ Web scraping techniques using BeautifulSoup
✅ Data cleaning and manipulation with Pandas
✅ Creating interactive visualizations with Plotly
✅ Analyzing real-world stock market data


📞 Contact
Author: [Abdulrahmans leem ]
Email: [abdulrahman.sleem@outlook.com]
LinkedIn: [https://www.linkedin.com/in/abdulrahmansleem/]

📅 Project Date
November 2025

📄 License
This project is part of an educational assignment and is available for learning purposes.

🙏 Acknowledgments

IBM Skills Network for providing the data sources
Yahoo Finance (yfinance) for stock data API
Plotly for visualization library


⭐ If you found this project helpful, please give it a star!
