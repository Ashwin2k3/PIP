- [ ] UNIT – IV
- [ ] Web Scraping:
- [ ]  Project: MAPIT.
- [ ] PY with the web browser Module,
- [ ]  Downloading Files from the Web with the requests Module,
- [ ]  Saving Downloaded Files to the Hard Drive,
- [ ]  HTML.

## WEB SCRAPPING
Web scraping in Python involves extracting data from websites. Here's a comprehensive guide to get you started:

**1. Understanding the Process:**

- **Target Website:** Identify the website you want to scrape data from.
- **Data Extraction:** Determine the specific data points you're interested in (text, images, links, etc.).
- **Website Structure:** Analyze the website's HTML structure to understand how the data is organized (tags, classes, IDs).

**2. Essential Python Libraries:**

- **`requests`:** To send HTTP requests to the website and retrieve the HTML content.
- **Beautiful Soup:** A popular library for parsing HTML content and extracting data. It simplifies navigating and searching within the HTML structure.
- **lxml:** Another powerful HTML parsing library (often faster than Beautiful Soup for very large websites).
- **Regular Expressions (optional):** For complex data extraction patterns (can be used with both Beautiful Soup and lxml).

**3. Basic Steps for Scraping:**

   ```python
   import requests
   from bs4 import BeautifulSoup  # Assuming you're using Beautiful Soup

   # 1. Send a GET request to the target URL
   url = "https://www.example.com"  # Replace with the actual website URL
   response = requests.get(url)

   # 2. Check for successful response
   if response.status_code == 200:
       # Parse the HTML content
       soup = BeautifulSoup(response.content, 'html.parser')

       # 3. Extract the data you need
       # Use techniques like finding elements by tag name, class, ID, or attributes
       data_elements = soup.find_all('div', class_='product-info')  # Hypothetical example

       # Process the extracted data elements (e.g., loop through them)
       for element in data_elements:
           title = element.find('h3').text  # Assuming title is within an h3 tag
           price = element.find('span', class_='price').text  # Assuming price is within a span with class 'price'
           print(f"Title: {title}, Price: {price}")

   else:
       print(f"Error: Failed to retrieve data. Status code: {response.status_code}")
   ```

**4. Important Considerations:**

- **Respect Robot.txt:** Websites might have a `robots.txt` file that specifies scraping guidelines. Be a responsible scraper and avoid overwhelming the website with requests.
- **Legal and Ethical Implications:** Ensure you have the right to scrape data from a website. Check their terms of service or contact them if unsure.
- **Dynamic Websites (optional):** Some websites use JavaScript to generate content dynamically. You might need additional libraries like Selenium to handle these cases.

**5. Tips and Best Practices:**

- Start with simpler websites to practice your scraping skills.
- Be specific in your data extraction logic to avoid scraping irrelevant content.
- Consider using libraries like `pandas` to organize and analyze the scraped data after extraction.
- Implement mechanisms to handle potential errors (e.g., website structure changes, missing elements).

**Additional Resources:**

- Beautiful Soup Documentation: [https://www.crummy.com/software/BeautifulSoup/bs4/doc/](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- Requests Documentation: [https://docs.python-requests.org/en/latest/](https://docs.python-requests.org/en/latest/)
- Web Scraping Tutorial: [https://realpython.com/tutorials/web-scraping/](https://realpython.com/tutorials/web-scraping/)

Remember, web scraping can be complex, so start small, experiment, and be respectful of the websites you scrape from.

## project MAPIT
Here's a Python project named MAPIT that allows you to search for a location using an address or zip code and displays it on a map using the web browser:

**1. Libraries:**

```python
import webbrowser
import requests  # For alternative geocoding if Google Maps API limit is reached
```

**2. User Input:**

```python
def get_location():
  """Prompts the user for a location (address or zip code) and returns it."""
  location = input("Enter a location (address or zip code): ")
  return location
```

**3. Geocoding (Using Google Maps API - Optional paid service):**

```python
def geocode_google_maps(location):
  """Uses Google Maps API to convert location to latitude and longitude.

  **Note:** This requires a Google Maps API key and may have usage limits.
  Replace 'YOUR_API_KEY' with your actual key and consider alternative methods
  (like using a free geocoding service from a different provider) if you
  encounter limitations.
  """
  api_key = 'YOUR_API_KEY'
  url = f"https://maps.googleapis.com/maps/api/geocode/json?address={location}&key={api_key}"
  response = requests.get(url)
  data = response.json()

  if response.status_code == 200 and data['status'] == 'OK':
    latitude = data['results'][0]['geometry']['location']['lat']
    longitude = data['results'][0]['geometry']['location']['lng']
    return latitude, longitude
  else:
    print(f"Geocoding failed with status: {data.get('status')}")
    return None, None
```

**4. Geocoding Alternative (Free Service):**

```python
def geocode_alternative(location):
  """Uses a free geocoding service as a backup (replace with your preferred service)."""
  # Example using OpenCageData (consider their usage limits)
  api_key = 'YOUR_ALTERNATIVE_API_KEY'
  url = f"https://api.opencagedata.com/geocode/v1/json?q={location}&key={api_key}"
  response = requests.get(url)
  data = response.json()

  if response.status_code == 200:
    if data['results']:
      latitude = data['results'][0]['geometry']['latitude']
      longitude = data['results'][0]['geometry']['longitude']
      return latitude, longitude
    else:
      print("Location not found using alternative service.")
  else:
    print(f"Geocoding failed (alternative) with status: {response.status_code}")
    return None, None
```

**5. Building the URL:**

```python
def build_map_url(latitude, longitude):
  """Constructs the URL for opening the location in Google Maps."""
  url = f"https://www.google.com/maps/@{latitude},{longitude},15z"
  return url
```

**6. Main Function:**

```python
def main():
  """Prompts for location, attempts geocoding, and opens the map in the web browser."""
  location = get_location()

  # Try Google Maps API first (if you have a key)
  latitude, longitude = geocode_google_maps(location)

  # If Google Maps API fails or has limits, use an alternative service
  if not latitude or not longitude:
    latitude, longitude = geocode_alternative(location)

  # If geocoding fails from both methods, inform the user
  if not latitude or not longitude:
    print("Geocoding failed for the provided location.")
    return

  map_url = build_map_url(latitude, longitude)
  webbrowser.open(map_url)

if __name__ == "__main__":
  main()
```

**Explanation:**

1. The `get_location()` function prompts the user for an address or zip code.
2. The `geocode_google_maps()` function (optional) uses the Google Maps API for geocoding (requires an API key). Consider usage limits and potential costs.
3. The `geocode_alternative()` function (optional) serves as a backup using a free geocoding service (replace with your preferred service).
4. The `build_map_url()` function constructs the URL for Google Maps based on the latitude and longitude.
5. The `main()` function ties everything together:
   - Gets the location from the user.
   - Attempts geocoding using Google Maps API (if)


## PY with the web browser Module
In Python, you can use the `webbrowser` module to easily open a web page in the user's default web browser. Here's a breakdown of its functionalities:

**Functionality:**

- Launches a web browser and navigates it to a specified URL.
- Can open a URL in a new window, new tab (if supported), or use the existing browser window.

**Basic Usage:**

```python
import webbrowser

url = "https://www.example.com"  # Replace with the actual URL you want to open
webbrowser.open(url)
```

This code opens the provided URL in the user's default web browser.

**Opening in a New Window (Optional):**

```python
webbrowser.open(url, new=2)  # 0: Same window (default), 1: New window, 2: New tab (if supported)
```

**Additional Considerations:**

- The `webbrowser` module interacts with the system's default web browser. You cannot control which browser is used.
- It's generally not recommended to rely on specific values for `new` (0, 1, 2) as behavior might vary across platforms and browsers.

**Example Project (Simple Web Search):**

```python
import webbrowser

def search_web(query):
  """Conducts a web search using a search engine and opens the results in the browser."""
  search_engine = "https://www.google.com/search?q="  # Replace with your preferred search engine
  url = f"{search_engine}{query}"
  webbrowser.open(url)

if __name__ == "__main__":
  search_term = input("Enter your search query: ")
  search_web(search_term)
```

This code prompts the user for a search query, constructs a URL using a search engine, and opens the results in the web browser.

Remember, the `webbrowser` module offers a convenient way to open web pages from your Python programs. Keep in mind its limitations and use it appropriately in your projects.

