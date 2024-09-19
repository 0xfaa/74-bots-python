# 74-bots python 

The code below describes how one could use the 74-bots api using python.

```python
import requests
import json
import time

BASE_URL = "https://api.74bots.com"

def register_user(username, password):
    response = requests.post(f"{BASE_URL}/register", json={"username": username, "password": password})
    return response.json()

def login_user(username, password):
    response = requests.post(f"{BASE_URL}/login", json={"username": username, "password": password})
    return response.json()

def verify_token(token):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.post(f"{BASE_URL}/verify", headers=headers)
    return response.json()

def link_x_account(token, x_username):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.post(f"{BASE_URL}/link-x-account", headers=headers, json={"x_username": x_username})
    return response.json()

def get_user_info(token):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.get(f"{BASE_URL}/me", headers=headers)
    return response.json()

def start_scraping_followers(token, authorization, csrf, cookie):
    headers = {"Authorization": f"Bearer {token}"}
    data = {
        "authorization": authorization,
        "csrf": csrf,
        "cookie": cookie
    }
    response = requests.post(f"{BASE_URL}/scrape-followers", headers=headers, json=data)
    return response.json()

def list_processes(token):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.get(f"{BASE_URL}/processes", headers=headers)
    return response.json()

def get_process_by_id(token, process_id):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.get(f"{BASE_URL}/processes/{process_id}", headers=headers)
    return response.json()

def detect_bots(token, process_id):
    headers = {"Authorization": f"Bearer {token}"}
    response = requests.get(f"{BASE_URL}/detect-bots/{process_id}", headers=headers)
    return response.json()

def main():
    # Register a new user
    register_result = register_user("testuser", "testpassword123")
    print("Register result:", json.dumps(register_result, indent=2))

    # Login
    login_result = login_user("testuser", "testpassword123")
    print("Login result:", json.dumps(login_result, indent=2))

    if login_result["success"]:
        token = login_result["token"]

        # Verify token
        verify_result = verify_token(token)
        print("Verify result:", json.dumps(verify_result, indent=2))

        # Link X account
        link_result = link_x_account(token, "x_test_username")
        print("Link X account result:", json.dumps(link_result, indent=2))

        # Get user info
        user_info = get_user_info(token)
        print("User info:", json.dumps(user_info, indent=2))

        # Start scraping followers (you need to provide valid X API credentials)
        # You can get those if you go to the dev-tools window while logged-in
        # in X dot com, then go to networking tab, filter fetch requests, and
        # inspect some request like all.json.
        #
        # ...or...
        #
        # Wait for Faa (@0x466161 btw) to release the chrome extension that does
        # that for you.
        scrape_result = start_scraping_followers(token, "Bearer ...", "...", "...")
        print("Start scraping result:", json.dumps(scrape_result, indent=2))

        if scrape_result["success"]:
            process_id = scrape_result["data"]["processId"]

            # Wait for the process to complete (in a real scenario, you might want to implement a more sophisticated waiting mechanism)
            time.sleep(60)

            # Get process by ID
            process_result = get_process_by_id(token, process_id)
            print("Process result:", json.dumps(process_result, indent=2))

            # Detect bots
            bots_result = detect_bots(token, process_id)
            print("Bot detection result:", json.dumps(bots_result, indent=2))

        # List all processes
        processes = list_processes(token)
        print("All processes:", json.dumps(processes, indent=2))

if __name__ == "__main__":
    main()
```
