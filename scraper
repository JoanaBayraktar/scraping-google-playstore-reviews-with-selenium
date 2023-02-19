from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import pandas as pd
from selenium.webdriver.common.action_chains import ActionChains
import time


driver = webdriver.Firefox()
adresse = "https://play.google.com/store/apps/details?id=de.rki.coronawarnapp&hl=de"
driver.get(adresse)
action = ActionChains(driver)


# access all comments
btn_all_entries = driver.find_element(By.XPATH, "/html/body/c-wiz[2]/div/div/div[1]/div[2]/div/div[1]/c-wiz[4]/section/div/div/div[5]/div/div/button/span")
btn_all_entries.click()


# scroll to the last user to load all comments to scape them
check_users = driver.find_elements(By.CLASS_NAME, "X5PpBb")
last_user = check_users[-1]
driver.execute_script("return arguments[0].scrollIntoView();", last_user)
time.sleep(2)
check_users_again = driver.find_elements(By.CLASS_NAME, "X5PpBb")


while len(check_users) != len(check_users_again):
    # get all visible users
    check_users = driver.find_elements(By.CLASS_NAME, "X5PpBb")
    # access the last visible user on page
    last_user = check_users[-1]
    # scroll to this user to load more content
    driver.execute_script("return arguments[0].scrollIntoView();", last_user)
    time.sleep(2)
    check_users_again = driver.find_elements(By.CLASS_NAME, "X5PpBb")
    for name in check_users:
        print(name.text)


# get the usernames
user_list = []
user_names = driver.find_elements(By.CLASS_NAME, "X5PpBb")
for i in range(len(user_names)):
    user_list.append(user_names[i].text)
df_user = pd.DataFrame(user_list, columns = ['username'])


# get the dates
dates_list = []
dates = driver.find_elements(By.CLASS_NAME, "bp9Aid")
for i in range(len(dates)):
    print(dates[i].text)
    dates_list.append(dates[i].text) #dates are available only as string
df_dates = pd.DataFrame(dates_list, columns = ['date'])


# get all (star)ratings
ratings_list = []
ratings = driver.find_elements(By.CLASS_NAME, "iXRFPc")
for i in range(len(ratings)):
    rate = ratings[i].get_attribute("aria-label")
    ratings_list.append(rate)
df_ratings = pd.DataFrame(ratings_list, columns = ['rating'])


# get comments
comments_list = []
comments = driver.find_elements(By.CLASS_NAME, "h3YV2d")
for i in range(len(comments)):
    print(comments[i].text)
    comments_list.append(comments[i].text)
df_comments = pd.DataFrame(comments_list, columns = ['text'])


# "was this comment usefull for your?" - Counter
usefull_list = []
usefull = driver.find_elements(By.CLASS_NAME, "AJTPZc")
for i in range(len(usefull)):
    print(usefull[i].text)
    usefull_list.append(usefull[i].text)
df_useful = pd.DataFrame(usefull_list, columns = ['useful'])


# get replies (if available)
replies_text_list = []
replies_text = driver.find_elements(By.CLASS_NAME, "ras4vb")
for i in range(len(replies_text)):
    print(replies_text[i].text)
    replies_text_list.append(replies_text[i].text)
df_replies_text = pd.DataFrame(replies_text_list, columns = ['reply_text'])


# get reply date
replies_date_list = []
replies_date = driver.find_elements(By.CLASS_NAME, "I9Jtec")
for i in range(len(replies_date)):
    print(replies_date[i].text)
    replies_date_list.append(replies_date[i].text) # only available as string
df_replies_date = pd.DataFrame(replies_date_list, columns = ['reply_date'])

# get review-id
id_list = []
id = driver.find_elements(By.CLASS_NAME, "c1bOId")
for i in range(len(id)):
    print(id[i].id)
    id_list.append(id[i].id)
df_id = pd.DataFrame(id_list, columns = ['id'])


# merge all dfs
dataframe = pd.concat([df_id, df_user, df_dates, df_ratings, df_comments, df_useful, df_replies_text, df_replies_date], axis = 1)


# save df to csv
dataframe.to_csv("cwa_de.csv")


# close the driver
driver.close()
