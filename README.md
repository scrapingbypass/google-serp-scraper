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
