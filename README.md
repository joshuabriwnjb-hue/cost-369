# cost-369
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
    
# ---------- SETUP ----------
driver = webdriver.Chrome()
wait = WebDriverWait(driver, 20)
driver.get("https://app.grabdocs.com/")
print("Browser opened.")
print(" Log in manually, then go to the MESSAGES page.")
input("Press ENTER here when you're ready...")
     
# ---------- SEND MESSAGE ----------
try:
    print("Sending message...")
        
    message_box = wait.until(
        EC.presence_of_element_located((By.TAG_NAME, "textarea"))
    )
    
    message_box.send_keys("Automated test message")
    message_box.send_keys(Keys.ENTER)
    
    print(" Message sent")
     
except Exception as e:
    print(" Message failed:", e)
    
    
# ---------- CLICK REACH ----------
try: 
    print("Opening Reach...")
    
    reach_button = wait.until(
        EC.element_to_be_clickable((By.XPATH, "//span[text()='Reach']"))
    )
    # scroll into view (fixes 'not interactable')
    driver.execute_script("arguments[0].scrollIntoView(true);", reach_button)
    time.sleep(1)
    reach_button.click()
    print(" Reach opened")
    print(" Reach opened")
except Exception as e:
    print(" Reach failed:", e)
# ---------- CREATE MEETING ----------
try:
    print("Creating meeting...")
    # wait for modal (overlay) to appear
    wait.until(
        EC.presence_of_element_located((By.CLASS_NAME, "fixed"))
    )
    # NOW find the Create button INSIDE the modal
    create_btn = wait.until(
        EC.element_to_be_clickable((By.XPATH, "//button[contains(.,'Create')]"))
    )
    
    driver.execute_script("arguments[0].scrollIntoView(true);", create_btn)
    driver.execute_script("arguments[0].click();", create_btn)
        
    # wait for meeting name input
    name_input = wait.until(
        EC.presence_of_element_located((By.XPATH, "//input[contains(@placeholder,'meeting')]"))
    )
    name_input.send_keys("Test Meeting")
    
    # final create click
    final_create = wait.until(
        EC.element_to_be_clickable((By.XPATH, "//button[contains(.,'Create')]"))
    )
    
    driver.execute_script("arguments[0].click();", final_create)
    print(" Meeting created")
    
except Exception as e:
    print(" Meeting creation failed:", e)
     
print("Automation complete")
time.sleep(10)
    
driver.quit()

