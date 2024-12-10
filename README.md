from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys  # Import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

# Initialize WebDriver
service = Service(r'C:\Users\DEEKSHA SRIVASTAVA\Downloads\chromedriver-win64\chromedriver-win64\chromedriver.exe')
driver = webdriver.Chrome(service=service)

def search_hotels():
    try:
        # Open the travel website
        driver.get("https://www.makemytrip.com/hotels/")
        driver.maximize_window()
        time.sleep(5)  # Allow initial page load

        # Handle the login/signup popup
        try:
            popup_close_button = WebDriverWait(driver, 10).until(
                EC.element_to_be_clickable((By.CSS_SELECTOR, ".modalClose"))
            )
            popup_close_button.click()  # Click the close button
            print("Popup closed successfully.")
        except Exception as e:
            print("No login/signup popup to close or error in closing:", e)

        # Click on the city field
        city_input = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.ID, "city"))
        )
        city_input.click()

        # Enter city name
        search_box = WebDriverWait(driver, 10).until(
            EC.presence_of_element_located((By.XPATH, "//input[@placeholder='Enter city/ Hotel/ Area/ Building']"))
        )
        search_box.send_keys("Goa")
        time.sleep(2)  # Wait for dropdown suggestions to appear
        search_box.send_keys(Keys.ARROW_DOWN)
        search_box.send_keys(Keys.ENTER)

        # Select check-in and check-out dates
        time.sleep(2)
        check_in_date = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.XPATH, "//div[@aria-label='Sat Dec 16 2023']"))
        )  # Replace with your desired date
        check_in_date.click()

        check_out_date = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.XPATH, "//div[@aria-label='Mon Dec 18 2023']"))
        )  # Replace with your desired date
        check_out_date.click()

        # Click the search button
        search_button = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable((By.ID, "hsw_search_button"))
        )
        search_button.click()
        time.sleep(5)  # Allow search results to load

        print("Hotel search automation completed successfully.")

    except Exception as e:
        print(f"An error occurred: {e}")

    finally:
        # Close the browser
        driver.quit()

if __name__ == "__main__":
    search_hotels()
