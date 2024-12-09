# Selenium-Script-for-Hotel-Search-Automation
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# Configure Chrome options
options = Options()
options.page_load_strategy = 'eager'

# Initialize the webdriver with options
driver = webdriver.Chrome(options=options)

# Go to MakeMyTrip
driver.get("https://www.makemytrip.com/")

# Maximize the window
driver.maximize_window()

# Close the login popup
try:
    popup_close = WebDriverWait(driver, 120).until(
        EC.element_to_be_clickable((By.CSS_SELECTOR, "li[data-cy='account'] span.chNavIcon.appendRight5"))
    )
    popup_close.click()
except:
    pass  # If popup doesn't appear, continue

# Find the "Hotels" tab and click it
hotels_tab = WebDriverWait(driver, 120).until(
    EC.element_to_be_clickable((By.XPATH, "//span[text()='Hotels']"))
)
hotels_tab.click()

# Enter city name (e.g., "New Delhi")
city_field = WebDriverWait(driver, 120).until(
    EC.presence_of_element_located((By.ID, "city"))
)
city_field.send_keys("New Delhi")
city_field.submit()  # Submit the form

# Wait for search results to load
WebDriverWait(driver, 120).until(
    EC.presence_of_element_located((By.CSS_SELECTOR, ".hotelCardListing"))
)

# Select the 5th hotel (index 4)
hotel_cards = driver.find_elements(By.CSS_SELECTOR, ".hotelCardListing")
if len(hotel_cards) >= 5:
    hotel_cards[4].click()
else:
    print("Not enough hotels found.")

# Switch to the new tab (hotel details page)
driver.switch_to.window(driver.window_handles[1])

# Wait for the room selection to load
WebDriverWait(driver, 120).until(
    EC.presence_of_element_located((By.CSS_SELECTOR, ".RoomBlock"))
)

# Choose the first available room
room_blocks = driver.find_elements(By.CSS_SELECTOR, ".RoomBlock")
if room_blocks:
    room_blocks[0].click()
else:
    print("No rooms found.")

# Further actions (e.g., select dates, book) can be added here

# Wait for a while to see the results
time.sleep(5)

# Close the browser
driver.quit()
