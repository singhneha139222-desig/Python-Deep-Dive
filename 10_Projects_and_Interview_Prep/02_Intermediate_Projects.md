# 📘 Intermediate Projects

## 📌 Introduction

These intermediate projects build on your foundation by incorporating APIs, web scraping, databases, and more complex OOP patterns. Each project is closer to real-world applications.

---

## 🎯 Project 1: Weather Dashboard

### Problem Statement

Create a weather application that:
- Fetches real-time weather data from API
- Displays current conditions and forecast
- Saves favorite cities
- Shows weather history

### Tech Stack
- `requests` for API calls
- `json` for data handling
- `sqlite3` for storage

### Complete Code

```python
import requests
import json
import sqlite3
from datetime import datetime
from typing import Optional, Dict, List

class WeatherAPI:
    """Handles weather API interactions"""
    
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.base_url = "http://api.openweathermap.org/data/2.5"
    
    def get_current_weather(self, city: str) -> Optional[Dict]:
        """Fetch current weather for a city"""
        try:
            url = f"{self.base_url}/weather"
            params = {
                "q": city,
                "appid": self.api_key,
                "units": "metric"  # Celsius
            }
            response = requests.get(url, params=params, timeout=10)
            response.raise_for_status()
            return response.json()
        except requests.RequestException as e:
            print(f"API Error: {e}")
            return None
    
    def get_forecast(self, city: str, days: int = 5) -> Optional[Dict]:
        """Fetch weather forecast"""
        try:
            url = f"{self.base_url}/forecast"
            params = {
                "q": city,
                "appid": self.api_key,
                "units": "metric",
                "cnt": days * 8  # 3-hour intervals
            }
            response = requests.get(url, params=params, timeout=10)
            response.raise_for_status()
            return response.json()
        except requests.RequestException as e:
            print(f"API Error: {e}")
            return None

class WeatherDatabase:
    """Handles database operations"""
    
    def __init__(self, db_name: str = "weather.db"):
        self.conn = sqlite3.connect(db_name)
        self.create_tables()
    
    def create_tables(self):
        cursor = self.conn.cursor()
        
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS favorites (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                city TEXT UNIQUE NOT NULL,
                country TEXT,
                added_at TEXT
            )
        ''')
        
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS history (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                city TEXT NOT NULL,
                temperature REAL,
                description TEXT,
                humidity INTEGER,
                wind_speed REAL,
                recorded_at TEXT
            )
        ''')
        
        self.conn.commit()
    
    def add_favorite(self, city: str, country: str = ""):
        try:
            cursor = self.conn.cursor()
            cursor.execute(
                "INSERT INTO favorites (city, country, added_at) VALUES (?, ?, ?)",
                (city, country, datetime.now().isoformat())
            )
            self.conn.commit()
            return True
        except sqlite3.IntegrityError:
            return False  # Already exists
    
    def get_favorites(self) -> List[Dict]:
        cursor = self.conn.cursor()
        cursor.execute("SELECT city, country FROM favorites")
        return [{"city": row[0], "country": row[1]} for row in cursor.fetchall()]
    
    def remove_favorite(self, city: str) -> bool:
        cursor = self.conn.cursor()
        cursor.execute("DELETE FROM favorites WHERE city = ?", (city,))
        self.conn.commit()
        return cursor.rowcount > 0
    
    def save_weather(self, city: str, data: Dict):
        cursor = self.conn.cursor()
        cursor.execute('''
            INSERT INTO history (city, temperature, description, humidity, wind_speed, recorded_at)
            VALUES (?, ?, ?, ?, ?, ?)
        ''', (
            city,
            data.get('temp'),
            data.get('description'),
            data.get('humidity'),
            data.get('wind_speed'),
            datetime.now().isoformat()
        ))
        self.conn.commit()
    
    def get_history(self, city: str = None, limit: int = 10) -> List[Dict]:
        cursor = self.conn.cursor()
        if city:
            cursor.execute(
                "SELECT * FROM history WHERE city = ? ORDER BY recorded_at DESC LIMIT ?",
                (city, limit)
            )
        else:
            cursor.execute(
                "SELECT * FROM history ORDER BY recorded_at DESC LIMIT ?",
                (limit,)
            )
        columns = ['id', 'city', 'temperature', 'description', 'humidity', 'wind_speed', 'recorded_at']
        return [dict(zip(columns, row)) for row in cursor.fetchall()]

class WeatherApp:
    """Main application class"""
    
    def __init__(self, api_key: str):
        self.api = WeatherAPI(api_key)
        self.db = WeatherDatabase()
    
    def display_weather(self, data: Dict):
        """Display formatted weather information"""
        if not data:
            print("❌ Unable to fetch weather data")
            return
        
        main = data.get('main', {})
        weather = data.get('weather', [{}])[0]
        wind = data.get('wind', {})
        sys = data.get('sys', {})
        
        print("\n" + "=" * 50)
        print(f"🌍 {data['name']}, {sys.get('country', 'Unknown')}")
        print("=" * 50)
        
        # Weather icon mapping
        icons = {
            'Clear': '☀️',
            'Clouds': '☁️',
            'Rain': '🌧️',
            'Snow': '❄️',
            'Thunderstorm': '⛈️',
            'Drizzle': '🌦️',
            'Mist': '🌫️',
            'Fog': '🌫️'
        }
        icon = icons.get(weather.get('main', ''), '🌡️')
        
        print(f"\n{icon} {weather.get('description', 'N/A').title()}")
        print(f"\n🌡️  Temperature: {main.get('temp', 'N/A')}°C")
        print(f"   Feels like: {main.get('feels_like', 'N/A')}°C")
        print(f"   Min: {main.get('temp_min', 'N/A')}°C | Max: {main.get('temp_max', 'N/A')}°C")
        print(f"\n💧 Humidity: {main.get('humidity', 'N/A')}%")
        print(f"💨 Wind: {wind.get('speed', 'N/A')} m/s")
        print(f"📊 Pressure: {main.get('pressure', 'N/A')} hPa")
        print("=" * 50)
        
        # Save to history
        self.db.save_weather(data['name'], {
            'temp': main.get('temp'),
            'description': weather.get('description'),
            'humidity': main.get('humidity'),
            'wind_speed': wind.get('speed')
        })
    
    def display_forecast(self, data: Dict):
        """Display weather forecast"""
        if not data:
            print("❌ Unable to fetch forecast data")
            return
        
        print("\n📅 5-DAY FORECAST")
        print("=" * 50)
        
        # Group by day
        daily = {}
        for item in data.get('list', []):
            date = item['dt_txt'].split()[0]
            if date not in daily:
                daily[date] = item
        
        for date, forecast in list(daily.items())[:5]:
            main = forecast.get('main', {})
            weather = forecast.get('weather', [{}])[0]
            print(f"\n📆 {date}")
            print(f"   {weather.get('description', 'N/A').title()}")
            print(f"   🌡️ {main.get('temp', 'N/A')}°C | 💧 {main.get('humidity', 'N/A')}%")
        
        print("\n" + "=" * 50)
    
    def show_favorites(self):
        """Display favorite cities"""
        favorites = self.db.get_favorites()
        
        if not favorites:
            print("\n⭐ No favorite cities yet!")
            return
        
        print("\n⭐ FAVORITE CITIES")
        print("=" * 30)
        for i, fav in enumerate(favorites, 1):
            print(f"{i}. {fav['city']}, {fav['country']}")
        print("=" * 30)
    
    def show_history(self):
        """Display weather history"""
        history = self.db.get_history(limit=10)
        
        if not history:
            print("\n📜 No weather history yet!")
            return
        
        print("\n📜 RECENT WEATHER SEARCHES")
        print("=" * 60)
        for record in history:
            time = record['recorded_at'][:16].replace('T', ' ')
            print(f"{time} | {record['city']}: {record['temperature']}°C - {record['description']}")
        print("=" * 60)
    
    def run(self):
        """Main application loop"""
        print("\n🌤️ WEATHER DASHBOARD")
        print("=" * 40)
        
        while True:
            print("\n📋 MENU")
            print("1. Search weather by city")
            print("2. View 5-day forecast")
            print("3. View favorite cities")
            print("4. Add city to favorites")
            print("5. Remove city from favorites")
            print("6. View search history")
            print("0. Exit")
            
            choice = input("\nEnter choice: ").strip()
            
            if choice == "1":
                city = input("Enter city name: ").strip()
                if city:
                    data = self.api.get_current_weather(city)
                    self.display_weather(data)
            
            elif choice == "2":
                city = input("Enter city name: ").strip()
                if city:
                    data = self.api.get_forecast(city)
                    self.display_forecast(data)
            
            elif choice == "3":
                self.show_favorites()
            
            elif choice == "4":
                city = input("Enter city to add: ").strip()
                if city:
                    # Verify city exists
                    data = self.api.get_current_weather(city)
                    if data:
                        country = data.get('sys', {}).get('country', '')
                        if self.db.add_favorite(data['name'], country):
                            print(f"⭐ Added {data['name']} to favorites!")
                        else:
                            print("Already in favorites!")
            
            elif choice == "5":
                self.show_favorites()
                city = input("Enter city to remove: ").strip()
                if city and self.db.remove_favorite(city):
                    print(f"🗑️ Removed {city} from favorites")
            
            elif choice == "6":
                self.show_history()
            
            elif choice == "0":
                print("👋 Goodbye!")
                break
            
            else:
                print("❌ Invalid choice!")

def main():
    # Get free API key from https://openweathermap.org/api
    API_KEY = "YOUR_API_KEY_HERE"  # Replace with your key
    
    if API_KEY == "YOUR_API_KEY_HERE":
        print("⚠️ Please get a free API key from openweathermap.org")
        print("   Replace 'YOUR_API_KEY_HERE' with your actual key")
        return
    
    app = WeatherApp(API_KEY)
    app.run()

if __name__ == "__main__":
    main()
```

### Skills Used
- API integration with `requests`
- SQLite database operations
- Error handling
- Data formatting and display
- OOP design patterns

### Improvements to Try
- Add weather alerts
- Implement caching
- Add graphical display with matplotlib
- Support multiple units (Fahrenheit/Celsius)

---

## 🎯 Project 2: Web Scraper

### Problem Statement

Build a web scraper that:
- Extracts data from websites
- Handles pagination
- Exports to CSV/JSON
- Respects robots.txt

### Tech Stack
- `requests` for HTTP
- `BeautifulSoup` for parsing
- `pandas` for data export

### Complete Code

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import json
import time
import re
from urllib.parse import urljoin, urlparse
from typing import List, Dict, Optional

class WebScraper:
    """General-purpose web scraper"""
    
    def __init__(self, base_url: str, delay: float = 1.0):
        self.base_url = base_url
        self.delay = delay  # Respectful delay between requests
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Educational Scraper Bot)'
        })
        self.data = []
    
    def fetch_page(self, url: str) -> Optional[BeautifulSoup]:
        """Fetch and parse a webpage"""
        try:
            time.sleep(self.delay)  # Be respectful
            response = self.session.get(url, timeout=10)
            response.raise_for_status()
            return BeautifulSoup(response.text, 'html.parser')
        except requests.RequestException as e:
            print(f"Error fetching {url}: {e}")
            return None
    
    def get_absolute_url(self, url: str) -> str:
        """Convert relative URL to absolute"""
        return urljoin(self.base_url, url)

class BookScraper(WebScraper):
    """Scraper for books.toscrape.com (practice site)"""
    
    def __init__(self):
        super().__init__("http://books.toscrape.com", delay=0.5)
    
    def scrape_book_list(self, page_url: str) -> List[Dict]:
        """Scrape list of books from a page"""
        soup = self.fetch_page(page_url)
        if not soup:
            return []
        
        books = []
        articles = soup.find_all('article', class_='product_pod')
        
        for article in articles:
            book = {}
            
            # Title
            title_tag = article.find('h3').find('a')
            book['title'] = title_tag.get('title', title_tag.text.strip())
            book['url'] = self.get_absolute_url(title_tag.get('href', ''))
            
            # Price
            price_tag = article.find('p', class_='price_color')
            book['price'] = price_tag.text.strip() if price_tag else 'N/A'
            
            # Rating
            rating_tag = article.find('p', class_='star-rating')
            rating_map = {'One': 1, 'Two': 2, 'Three': 3, 'Four': 4, 'Five': 5}
            for rating, value in rating_map.items():
                if rating in rating_tag.get('class', []):
                    book['rating'] = value
                    break
            
            # Availability
            stock_tag = article.find('p', class_='instock')
            book['in_stock'] = 'In stock' in stock_tag.text if stock_tag else False
            
            books.append(book)
        
        return books
    
    def scrape_book_details(self, book_url: str) -> Dict:
        """Scrape detailed info from book page"""
        soup = self.fetch_page(book_url)
        if not soup:
            return {}
        
        details = {}
        
        # Title
        title = soup.find('h1')
        details['title'] = title.text.strip() if title else 'N/A'
        
        # Description
        desc_tag = soup.find('article', class_='product_page')
        desc = desc_tag.find_all('p')[-1] if desc_tag else None
        details['description'] = desc.text.strip() if desc else 'N/A'
        
        # Product info table
        table = soup.find('table', class_='table')
        if table:
            for row in table.find_all('tr'):
                header = row.find('th').text.strip()
                value = row.find('td').text.strip()
                details[header.lower().replace(' ', '_')] = value
        
        return details
    
    def scrape_all_books(self, max_pages: int = 5) -> List[Dict]:
        """Scrape books from multiple pages"""
        all_books = []
        page_num = 1
        
        while page_num <= max_pages:
            if page_num == 1:
                url = f"{self.base_url}/catalogue/page-{page_num}.html"
            else:
                url = f"{self.base_url}/catalogue/page-{page_num}.html"
            
            print(f"📖 Scraping page {page_num}...")
            books = self.scrape_book_list(url)
            
            if not books:
                print("No more pages found.")
                break
            
            all_books.extend(books)
            page_num += 1
        
        return all_books
    
    def scrape_by_category(self, category_url: str) -> List[Dict]:
        """Scrape all books in a category"""
        books = []
        page_url = category_url
        
        while page_url:
            soup = self.fetch_page(page_url)
            if not soup:
                break
            
            # Get books on current page
            books.extend(self.scrape_book_list(page_url))
            
            # Check for next page
            next_btn = soup.find('li', class_='next')
            if next_btn:
                next_link = next_btn.find('a').get('href')
                page_url = self.get_absolute_url(next_link)
            else:
                page_url = None
        
        return books

class DataExporter:
    """Export scraped data to various formats"""
    
    @staticmethod
    def to_csv(data: List[Dict], filename: str):
        """Export to CSV"""
        df = pd.DataFrame(data)
        df.to_csv(filename, index=False)
        print(f"✅ Exported {len(data)} records to {filename}")
    
    @staticmethod
    def to_json(data: List[Dict], filename: str):
        """Export to JSON"""
        with open(filename, 'w') as f:
            json.dump(data, f, indent=2)
        print(f"✅ Exported {len(data)} records to {filename}")
    
    @staticmethod
    def to_excel(data: List[Dict], filename: str):
        """Export to Excel"""
        df = pd.DataFrame(data)
        df.to_excel(filename, index=False)
        print(f"✅ Exported {len(data)} records to {filename}")

def main():
    print("\n🕷️ WEB SCRAPER")
    print("=" * 40)
    
    scraper = BookScraper()
    
    while True:
        print("\n📋 MENU")
        print("1. Scrape book listings (multiple pages)")
        print("2. Scrape single book details")
        print("3. Export last scrape to CSV")
        print("4. Export last scrape to JSON")
        print("0. Exit")
        
        choice = input("\nEnter choice: ").strip()
        
        if choice == "1":
            pages = input("How many pages to scrape? (default 3): ").strip()
            pages = int(pages) if pages.isdigit() else 3
            
            print(f"\n🔍 Scraping {pages} pages...")
            scraper.data = scraper.scrape_all_books(max_pages=pages)
            
            print(f"\n📊 Scraped {len(scraper.data)} books:")
            for i, book in enumerate(scraper.data[:5], 1):
                print(f"{i}. {book['title'][:40]}... - {book['price']}")
            if len(scraper.data) > 5:
                print(f"   ... and {len(scraper.data) - 5} more")
        
        elif choice == "2":
            url = input("Enter book URL (or 'test' for sample): ").strip()
            if url.lower() == 'test':
                url = "http://books.toscrape.com/catalogue/a-light-in-the-attic_1000/index.html"
            
            details = scraper.scrape_book_details(url)
            if details:
                print("\n📖 BOOK DETAILS")
                print("=" * 40)
                for key, value in details.items():
                    if key == 'description':
                        print(f"{key}: {value[:100]}...")
                    else:
                        print(f"{key}: {value}")
        
        elif choice == "3":
            if scraper.data:
                filename = input("Filename (default: books.csv): ").strip() or "books.csv"
                DataExporter.to_csv(scraper.data, filename)
            else:
                print("❌ No data to export. Run a scrape first!")
        
        elif choice == "4":
            if scraper.data:
                filename = input("Filename (default: books.json): ").strip() or "books.json"
                DataExporter.to_json(scraper.data, filename)
            else:
                print("❌ No data to export. Run a scrape first!")
        
        elif choice == "0":
            print("👋 Goodbye!")
            break
        
        else:
            print("❌ Invalid choice!")

if __name__ == "__main__":
    main()
```

### Skills Used
- Web scraping with BeautifulSoup
- HTTP sessions and headers
- Data parsing and extraction
- CSV/JSON export
- Pagination handling

### Improvements to Try
- Add proxy support
- Implement concurrent scraping
- Add data validation
- Create CLI arguments

---

## 📝 Skills Checklist

After completing intermediate projects, you should be able to:

- [ ] Integrate with external APIs
- [ ] Parse HTML/JSON data
- [ ] Work with databases
- [ ] Handle errors in complex scenarios
- [ ] Export data to multiple formats
- [ ] Build modular, maintainable code

---

## ⏭️ Next: Advanced Projects

Ready for production-level code? Move on to **[Advanced Projects](03_Advanced_Projects.md)** to build full applications!

---

*Build real tools, solve real problems!* 🛠️
