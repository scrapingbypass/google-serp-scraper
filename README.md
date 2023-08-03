## Google SERP scraper
Google [SERP scraper](https://www.scrapingbypass.com/serp-scraper-api.html) code with Python
```
# SERP scraper
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

# Set up WebDriver
options = webdriver.ChromeOptions()
options.add_argument("--start-maximized")
driver = webdriver.Chrome(options=options)

# Load Google search page
url = 'https://www.google.com/'
driver.get(url)

# Search for keyword
search_box = driver.find_element(By.NAME, 'q')
search_term = 'scrapingbypass'
search_box.send_keys(search_term)
search_box.send_keys(Keys.RETURN)

num = 1

# Scrape multiple pages
for page in range(1, 6):  # Scrape the first 5 pages of results
    # Wait for the search results page to load
    try:
        element_present = EC.presence_of_element_located((By.CSS_SELECTOR, '.g'))
        WebDriverWait(driver, 10).until(element_present)
    except TimeoutException:
        print("Timed out waiting for page to load")

    # Parse the search results
    search_results = driver.find_elements(By.CSS_SELECTOR, '.g')
    for result in search_results:
        link = result.find_element(By.CSS_SELECTOR, 'a').get_attribute('href')
        title = result.find_element(By.CSS_SELECTOR, 'h3').text
        print(num)
        print(title)
        print(link)
        num = num + 1

    # Click on the next page
    try:
        next_button = driver.find_element(By.CSS_SELECTOR, '#pnnext')
        next_button.click()
    except:
        break

# Close the WebDriver
driver.quit()
```

Or
```
import requests
from bs4 import BeautifulSoup

# Search keywords
search_term = "scrapingbypass"

# Request headers
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36"}

# Search pages of results
num_pages = 5
no=1

for page in range(0,num_pages):
    # Request URL
    if page == 0:
        url = f"https://www.google.com/search?q={search_term}"
    else:
        url = f"https://www.google.com/search?q={search_term}&start={page*10}"

    # Request
    response = requests.get(url, headers=headers)

    # Parse HTML
    soup = BeautifulSoup(response.content, "html.parser")

    # Extract search reult
    search_results = soup.select(".yuRUbf")

    # Print title and link
    for result in search_results:
        title = result.select_one("h3").text
        link = result.select_one("a")["href"]
        print(f"{no}: {title}: {link}")
        no=no+1
```
