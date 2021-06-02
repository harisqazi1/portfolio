# Shodan Search

I wanted to make a search for Shodan using code. This led me to create the following the python code that way I can use the API to search for specific strings without having to search up the specific command. There are a lot of security cameras in my Shodan code. I just thought this would be interesting have.

{% hint style="danger" %}
Use this code at your own risk. I am not responsible for what you do with this code.
{% endhint %}

```python
#!/bin/env python3

script_logo = """ _____ _____ _____ ____  _____ _____    _____ _____ _____ _____ _____ _____
|   __|  |  |     |    \|  _  |   | |  |   __|   __|  _  | __  |     |  |  |
|__   |     |  |  |  |  |     | | | |  |__   |   __|     |    -|   --|     |
|_____|__|__|_____|____/|__|__|_|___|  |_____|_____|__|__|__|__|_____|__|__|
"""
import shodan
import sys

# Setup the api
api = shodan.Shodan('Enter Your Key here') #api-key

def shodan_search():
    shodan_input = '' #What the API will search on shodan
    print('[1] VNC (Authentiction Disabled)')
    print('[2] IP Webcam')
    print('[3] webcamxp') 
    print('[4] FTP login')
    print('[5] NETSurveillance WEB')
    print('[6] Security Spy Camera')
    print('[7] Lilin IP Cameras')
    print('[8] SCADA modbus in port 502')
    user_choice = int(input("What is your choice?: "))
    if user_choice == 1:
        shodan_input = 'port:5901 authentication disabled'
    elif user_choice == 2:
        shodan_input = 'title:"IP Webcam"'
    elif user_choice == 3:
        shodan_input = 'webcamxp'
    elif user_choice == 4:
        shodan_input = '"220" "230 Login successful." port:21'
    elif user_choice == 5:
        shodan_input = 'title:"NETSurveillance WEB"'
    elif user_choice == 6:
        shodan_input = 'title:SecuritySpy'
    elif user_choice == 7:
        shodan_input = 'WWW-Authenticate: Basic realm="Merit LILIN Ent. Co., Ltd."'
    elif user_choice == 8:
        shodan_input = 'port 502'
    else:
        print("You did not choose the right choice")
        sys.exit(1)

    print('How many outputs do you want?')
    number_of_results = int(input("Enter your choice: "))

    try:
        # Perform the search
        result = api.search(shodan_input, limit=number_of_results)
        # Loop through the matches and print each IP
        print("----------------------------------------------------------------------")
        print("                                 RESULTS                              ")
        print("----------------------------------------------------------------------")
        for service in result['matches']:
            IP = service['ip_str']
            COUNTRY = service['location']['country_name']
            PORT = service['port']
            DATA = service['data']
            port = str(PORT)
            print("IP Address: " + IP)
            print("COUNTRY/LOCATION: " + COUNTRY)
            print("PORT: " + port)
            if user_choice == 2:
                print("Video location:  " + 'http://' + IP +':'+port+'/video')
            if user_choice == 7:
                print("Video Location:  " + 'http://' + IP +':'+port+'/lang1/index.html')
            print("DATA: " + DATA)

    except Exception as e:
        print('Error: %s' % e)
        sys.exit(1)


if __name__ == "__main__":
    print(script_logo)
    shodan_search()
    print('-----------------Script DONE------------')
```

## Resources I used:

* [https://help.shodan.io/guides/how-to-download-data-with-api](https://help.shodan.io/guides/how-to-download-data-with-api)
* [https://github.com/random-robbie/My-Shodan-Scripts](https://github.com/random-robbie/My-Shodan-Scripts)
* [https://www.yeahhub.com/find-vulnerable-webcams-shodan-metasploit-framework/](https://www.yeahhub.com/find-vulnerable-webcams-shodan-metasploit-framework/)
* [https://thedarksource.com/shodan-cheat-sheet/](https://thedarksource.com/shodan-cheat-sheet/)

