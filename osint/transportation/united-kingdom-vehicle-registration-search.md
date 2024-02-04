# United Kingdom Vehicle Registration Search

I recently got introduced to how vehicles are registered in the UK. I learned a lot while looking into this topic. This blog post will be a mix of two topics: a summary of how the plates are registered and a script to brute-force unknown vehicle registration numbers.

## Vehicle Registration

{% hint style="info" %}
I will be summarizing mostly from the "Vehicle registration numbers and number plates" (INF104 Vehicle Services) PDF by the government of U.K. (Source #1)
{% endhint %}

The "registration numbers" (which also include the letter values) are a way to identify vehicles that are owned by the Secretary of State. According to the government of U.K., "The registration number is given to the vehicle, not the registered keeper. It will stay with the vehicle (until the vehicle is broken up, destroyed or exported permanently out of the country) unless the registered keeper applies to take it off and put it on another vehicle or on to a retention certificate (V778)". The current format was introduced on September 1, 2001. It consists of:

* 2 letter memory tag (these refer to the region in the country where a vehicle is first registered)
  * "I" (capital "i"), "Q", and "Z" are not used in local memory tags
* 2 numbers (these tell you when it was issued)
* a space and 3 letters chosen at random
  * "Z" can be used as a random letter

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The memory tags can tell us what region the vehicle was first registered in. Using the example above, "BD" would tell us the vehicle was registered in Birmingham. For the age, "51" would tell us it was registered between Sept 2001 and Feb 2002. The last three values are just random letters. The PDF listed in the Sources section below has a great cheat-sheet for all of the regions where this is applicable. With this information about a vehicle, as an OSINT Analyst, we can make assumptions about where an individual lives based on their vehicle registration. However, there is the possibility of a person buying a vehicle registered in another region.&#x20;

From an OSINT perspective, there will be times when information is limited or redacted, and we must make an assumption or educated guess to move the investigation further. For example, using the registration number above, imagine if we were given only the first six values:  `BD51 SM`. We do not know the last value. Based on our research, we can assume it is an alphabetical letter between A and Z, but we cannot say which one for sure. Using code to automate this process, we can quickly generate all 26 possible values for the registration number and check those with the U.K. Driver & Vehicle Licensing Agency's database. There is a website that I had found that does this well: [https://www.partialnumberplate.co.uk/](https://www.partialnumberplate.co.uk/). However, I did not find many other websites like this, so I decided to write my own code to automate this process.

## Registration Number Wildcard Search

The code I had written works, but it definitely is not beginner-friendly. I will show the code first, then describe what it does.&#x20;

```python
#!/usr/bin/python3
import string
import requests
import time

API_key = "API_KEY_HERE" #Enter your API key here
#format for vehicles is LLNN LLL (L=Letter; N=Number)
first_letter = list(string.ascii_uppercase)
#DVLA does not use I, Q, or Z in memory tags
first_letter.remove("I")
first_letter.remove("Q") #might have to remove
first_letter.remove("Z")
second_letter = list(string.ascii_uppercase)
second_letter.remove("I")
second_letter.remove("Q") #might have to remove
second_letter.remove("Z")
third_digit = list(string.digits) # remove 3,4,8,9 as they currently cannot be possible values
third_digit.remove("3")
third_digit.remove("4")
third_digit.remove("8")
third_digit.remove("9")
fourth_digit = list(string.digits)
fifth_letter = list(string.ascii_uppercase)
sixth_letter = list(string.ascii_uppercase)
seventh_letter = list(string.ascii_uppercase)

#Tracking the amount of wildcards in the plate - max 3 currently
alphabet_wildcard = 0
number_wildcard = 0

# Can be used to output a list of all possible values as well - might have to dedup after 
def full():
    # license_plate = "@@$$@@@" #Removed space to make it easier to use with API; @ for letter, $ for number
    # used symbols to mitigate the issue of overwriting true positive license characters
    license_plate = "@@$$@@@" #All letters and numbers are unknown; NOT Recommended.
    for first in first_letter:
        for second in second_letter:
            for third in third_digit:
                for fourth in fourth_digit:
                    for fifth in fifth_letter: 
                        for sixth in sixth_letter: 
                            for seventh in seventh_letter: 
                                print(license_plate.replace("@", first, 1).replace("@", second, 1).replace("$", third, 1).replace("$", fourth, 1).replace("@", fifth, 1).replace("@", sixth, 1).replace("@", seventh, 1))

license_plate = "AA1$AAA" #Removed space to make it easier to use with API; @ for letter, $ for number

def format_check():
    #Simple check; can be expanded to check each position, but deemed not necessary
    if len(license_plate) != 7:
        print("License Plate Size Incorrect")
        exit()
    
def wildcards():
    # Using global to be able to modify the variables outside of the function
    global alphabet_wildcard
    global number_wildcard
    #Search for "@" and "$" in licence plate to see how many wildcards to work off of
    for index in license_plate:
        if index == "@":
            alphabet_wildcard += 1
        if index == "$":
            number_wildcard += 1

#Cue the API to output information regarding a specific registration number
def license_print():
    global alphabet_wildcard
    global number_wildcard
    # These do NOT account for the I,Q,Z not referenced in the memory tags - TLDR; extra output
    if alphabet_wildcard == 1 and number_wildcard == 0:
        for upper in list(string.ascii_uppercase):
            print("Post request submitted for: {}".format(license_plate.replace("@", upper)))
            api_request(license_plate.replace("@", upper))
    elif alphabet_wildcard == 0 and number_wildcard == 1:
        for digit in list(string.digits):
            print("Post request submitted for: {}".format(license_plate.replace("$", digit)))
            api_request(license_plate.replace("$", digit))
    elif alphabet_wildcard == 1 and number_wildcard == 1:
        for upper in list(string.ascii_uppercase):
            for digit in list(string.digits):
                print("Post request submitted for: {}".format(license_plate.replace("@", upper).replace("$", digit)))
                api_request(license_plate.replace("@", upper).replace("$", digit))
    elif alphabet_wildcard == 2 and number_wildcard == 1:
        for first in list(string.ascii_uppercase):
            for second in list(string.ascii_uppercase):
                for digit in list(string.digits):
                    print("Post request submitted for: {}".format(license_plate.replace("@", first, 1).replace("@", second,1).replace("$", digit)))
                    api_request(license_plate.replace("@", first, 1).replace("@", second,1).replace("$", digit))
    elif alphabet_wildcard == 1 and number_wildcard == 2:
        for upper in list(string.ascii_uppercase):
            for first in list(string.digits):
                for second in list(string.digits):
                    print("Post request submitted for: {}".format(license_plate.replace("@", upper, 1).replace("$", first,1).replace("$", second, 1)))
                    api_request(license_plate.replace("@", upper, 1).replace("$", first,1).replace("$", second, 1))
    elif alphabet_wildcard == 3 and number_wildcard == 0:
        for first in list(string.ascii_uppercase):
            for second in list(string.ascii_uppercase):
                for third in list(string.ascii_uppercase):
                    print("Post request submitted for: {}".format(license_plate.replace("@", first, 1).replace("@", second,1).replace("@", third, 1)))
                    api_request(license_plate.replace("@", first, 1).replace("@", second,1).replace("@", third, 1))
    elif alphabet_wildcard == 0 and number_wildcard == 3:
        for first in list(string.digits):
            for second in list(string.digits):
                for third in list(string.digits):
                    print("Post request submitted for: {}".format(license_plate.replace("$", first, 1).replace("$", second,1).replace("$", third, 1)))
                    api_request(license_plate.replace("$", first, 1).replace("$", second,1).replace("$", third, 1))
    #not working for some reason
    else:
        ("Currently not supported")
        exit()

def api_request(reg_num):
    # Actual Environment
    #url = 'https://driver-vehicle-licensing.api.gov.uk/vehicle-enquiry/v1/vehicles'
    # Test Environment
    url = 'https://uat.driver-vehicle-licensing.api.gov.uk/vehicle-enquiry/v1/vehicles'
    payload = "{\n\t\"registrationNumber\": \"%(reg)s\"\n}" % {'reg': reg_num} # String interpolation with %
    headers = {
    'x-api-key': API_key,
    'Content-Type': 'application/json'
    }
    response = requests.request("POST", url, headers=headers, data = payload)
    #Using print(response.text.encode('utf8')) outputs lines with b' which then needs .decode("utf-8") to fix
    print(response.text)
    time.sleep(2) # 2 second time out to prevent flooding
            
if __name__ == '__main__':
    format_check()
    #full()
    wildcards()
    license_print()
```

There are two functions that print out the registration numbers: full and license\_print. The full function is disabled by default, but outputs all possible values for the registration numbers. It takes a while to output all. I kept it there as a proof of concept of how to get all of the values. license\_print is there to output the registration numbers based on wildcards. In the code, I use "$" for a numerical wildcard, and a "@" for an alphabetical wildcard. **The code will not work as expected until you put in an API key at the top.** I requested an API key from the DVLA (currently located at: [https://register-for-ves.driver-vehicle-licensing.api.gov.uk/](https://register-for-ves.driver-vehicle-licensing.api.gov.uk/)) and got the key overnight. Keep in mind, the more alphabetical wildcards there are, the more output there will be. **The code currently is set up to query the test API, so un-comment the regular API to query actual data. You will then have to comment out the test API line.** Below is output I got from the test API. I know the value of "AA19AAA" would give me a result, so I made the position of the "9" to be the wildcard: `AA1$AAA`. This wildcard would go from `AA11AAA` to `AA19AAA`. We can see this in the following output of the code:

```json
Post request submitted for: AA10AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA11AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA12AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA13AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA14AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA15AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA16AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA17AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA18AAA
{"errors":[{"status":"404","code":"404","title":"Vehicle Not Found","detail":"Record for vehicle not found"}]}
Post request submitted for: AA19AAA
{"registrationNumber":"AA19AAA","artEndDate":"2025-03-30","co2Emissions":300,"engineCapacity":2000,"euroStatus":"EURO1","markedForExport":false,"fuelType":"PETROL","motStatus":"No details held by DVLA","revenueWeight":0,"colour":"RED","make":"FORD","typeApproval":"M1","yearOfManufacture":2019,"taxDueDate":"2024-07-31","taxStatus":"Taxed","dateOfLastV5CIssued":"2019-05-20","wheelplan":"2 AXLE RIGID BODY","monthOfFirstDvlaRegistration":"2019-03","monthOfFirstRegistration":"2019-03","realDrivingEmissions":"1"}
```

If you want to use `jq` to make the output look much cleaner, you can remove the print statements. There are a lot of guardrails I contemplated on including in this code. If I was to include the guardrails for the positions of the wildcards to be correct and making dictionaries for all combinations of the first two letters, that would end up taking a lot of lines of code. I do have that at the back of mind, and might consider it, if time and interest permits.&#x20;

## Code Limitations

The code is limited to 3 wildcards. I felt like any more than 3 wildcards, and you are just brute-forcing a government portal without any actual direction of what you are looking for. My code is a bit on the greedier side. It outputs more than is needed. In instances where a wildcard is one of the first 2 letters, it would print out all letters from A-Z, without excluding the letters not used: "I", "Q", "Z". If I ever do a revisit to this code, or a redo, I plan on making it exclude those values and be able to be more precise in the output. My goal was to make a code that just works. I was not able to find many resources that did just this, so I decided to make code that .... well, just works.&#x20;

## Updated Code

```python
# Import modules
import requests
import time
import sys
import string
import argparse
import signal # To catch the KeyboardInterrupt when CTRL-C is run

#Parsing the command-line arguments
parser = argparse.ArgumentParser(
                    prog='licenseV2.py',
                    description='This code uses UK partial plates to convert that into potential plates. It then searches the DVLA API with that information.',
                    epilog='Created by: Haris Qazi. Assisted by: AccessOSINT.')
parser.add_argument("license", type=str)
parser.add_argument('--api', help='Use API Key. If no key is provided, it will not search DVLA database.') 
args = parser.parse_args()
# Plate Values
memory_tag =  list(string.ascii_uppercase)
memory_tag.remove("I")
memory_tag.remove("Q")
memory_tag.remove("Z")
third_number = ("0", "1", "2", "5", "6", "7")
# fourth number is done in the loops themselves
random_letter = list(string.ascii_uppercase)
random_letter.remove("I")
random_letter.remove("Q")

# The following follow a "waterfall effect". wildcards_plates -> wildcard_plates -> plates. The wildcard plates get placed on the top and then one by one the wildcards are removed.
wildcards_plates = [] # 3 wildcards -> which turn to 2
wildcard_plates = [] # 2 wildcards -> which turn to 1
plates = [] # Completed List; 1, which turn to 0
# Command Line Input
partial = args.license.upper().replace(" ", "")
# Gets the indexes of where wildcard characters are.
# uses list compression
indices = [i for i, char in enumerate(partial) if char == '?']
#count the amount of wildcards
wildcard = len(indices)
# Check
if args.license == "":
    print("No argument provided")
    exit()
elif len(partial) != 7:
    print("Size Incorrect")
    exit()
elif "?" not in partial:
    print("No Wildcards found")
    exit()
elif wildcard > 3 or wildcard < 1:
    print("Only 1 - 3 Wildcards Allowed")
    exit()
# ---------------------------------------------------------------------------------------------
def one_wildcard():
# Works for ?D51AAA and B?51AAA
    if indices[0] < 2:
        for letter in memory_tag:
            wildcards_plates.append(partial.replace("?", letter, 1))
    # BD?1AAA
    elif indices[0] == 2:
        for number in third_number:
            wildcards_plates.append(partial.replace("?", number, 1))

    # BD5?AAA
    elif indices[0] == 3:
        if partial[2] == "0":
            for number in range(2, 10):
                wildcards_plates.append(partial.replace("?", str(number), 1))
        elif partial[2] == "2" or partial[2] == "7":
            for number in range(1, 4): #Until February 2024
                wildcards_plates.append(partial.replace("?", number, 1))
        else:
            for number in list(string.digits): #could have done a set, but would be overkill
                wildcards_plates.append(partial.replace("?", number, 1))
    # BD51?AA and BD51A?A and BD51AA?
    elif indices[0] > 3:
        for letter in random_letter:
            wildcards_plates.append(partial.replace("?", letter, 1))
# ---------------------------------------------------------------------------------------------
def two_wildcards():
    for p in wildcards_plates:
        if indices[1] < 2:
            for letters in memory_tag:
                #print(p.replace("?", letters))
                wildcard_plates.append(p.replace("?", letters, 1))
        # BD?1AAA
        elif indices[1] == 2:
            for number in third_number:
                wildcard_plates.append(p.replace("?", number, 1))
        elif indices[1] == 3:
            if p[2] == "0":
                for number in range(2, 10):
                    wildcard_plates.append(p.replace("?", str(number), 1))
            elif p[2] == "2" or p[2] == "7":
                for number in range(1, 4): #Until February 2024
                    wildcard_plates.append(p.replace("?", str(number), 1))
            else:
                for number in list(string.digits): #could have done a set, but would be overkill
                    wildcard_plates.append(p.replace("?", number, 1))
        else:
            for letter in random_letter:
                wildcard_plates.append(p.replace("?", letter, 1))
# ---------------------------------------------------------------------------------------------
def three_wildcards():
    for p in wildcard_plates:
        if indices[2] < 2:
            for letters in memory_tag:
                #print(p.replace("?", letters))
                plates.append(p.replace("?", letters, 1))
        elif indices[2] == 2:
            for number in third_number:
                plates.append(p.replace("?", number, 1))
        elif indices[2] == 3:
            if p[2] == "0":
                for number in range(2, 10):
                    plates.append(p.replace("?", str(number), 1))
            elif p[2] == "2" or p[2] == "7":
                for number in range(1, 4): #Until February 2024
                    plates.append(p.replace("?", str(number), 1))
            else:
                for number in list(string.digits): #could have done a set, but would be overkill
                    plates.append(p.replace("?", number, 1))
        else:
            for letter in random_letter:
                plates.append(p.replace("?", letter, 1))        
# ---------------------------------------------------------------------------------------------
def api_request(reg_num):
    # Actual Environment
    #url = 'https://driver-vehicle-licensing.api.gov.uk/vehicle-enquiry/v1/vehicles'
    # Test Environment
    url = 'https://uat.driver-vehicle-licensing.api.gov.uk/vehicle-enquiry/v1/vehicles'
    payload = "{\n\t\"registrationNumber\": \"%(reg)s\"\n}" % {'reg': reg_num} # String interpolation with %
    headers = {
    'x-api-key': args.api, #using the argument for --api
    'Content-Type': 'application/json'
    }
    response = requests.request("POST", url, headers=headers, data = payload)
    #Using print(response.text.encode('utf8')) outputs lines with b' which then needs .decode("utf-8") to fix
    print(response.text)
    #time.sleep(2) # 2 second time out to prevent flooding
# ---------------------------------------------------------------------------------------------
# Not Needed - felt like it would make the output clean
def sigint_handler(signal, frame):
    print ('KeyboardInterrupt Error Caught')
    sys.exit(0)

def main():
    global plates
    signal.signal(signal.SIGINT, sigint_handler)
    if wildcard == 1:
        one_wildcard()
        plates = wildcards_plates
    elif wildcard == 2:
        one_wildcard()
        two_wildcards()
        plates = wildcard_plates
    else:
        one_wildcard()
        two_wildcards()
        three_wildcards()
        #plates = * not needed, as we are writing to plates variable directly
    # Check if API key is provided here
    if args.api is not None:
        for plate in plates:
            api_request(plate)
    else:
        for plate in plates:
            print(plate)

if __name__ == "__main__":
    main()
```

To run the code, simply run `python3 licenseV2.py "license_plate"`, where you replace the string with the partial plate with the wildcard `?`. An example of this would be `python3 licenseV2.py "AA1?AAA"` on Linux. This would output all possibilites for the 4th value. If you have an API key and would like to use it, use the `--api` argument and provide the key in quotes after that, such as: `python3 licenseV2.py "AA1?AAA" --api "ThisIsMyAPIKey"`. If the key is provided, the DVLA API will be queried. If not, the plates will be printed out.

Here are the following changes updated in this version of the code:

* Replacing the wildcard from `$` and `@` to just `?`
* Command line arguments for the registration number and the API key
* Adding `--help` information to guide users
* Changing the code from the ground up

## Source

* [https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment\_data/file/1068365/inf104-vehicle-registration-numbers-and-number-plates.pdf](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment\_data/file/1068365/inf104-vehicle-registration-numbers-and-number-plates.pdf)
* [https://developer-portal.driver-vehicle-licensing.api.gov.uk/apis/vehicle-enquiry-service/code-examples.html#nodejs-using-axios](https://developer-portal.driver-vehicle-licensing.api.gov.uk/apis/vehicle-enquiry-service/code-examples.html#nodejs-using-axios)
* [https://developer-portal.driver-vehicle-licensing.api.gov.uk/apis/vehicle-enquiry-service/vehicle-enquiry-service-description.html#vehicle-enquiry-service-ves-api-guide](https://developer-portal.driver-vehicle-licensing.api.gov.uk/apis/vehicle-enquiry-service/vehicle-enquiry-service-description.html#vehicle-enquiry-service-ves-api-guide)
* [https://en.wikipedia.org/wiki/Vehicle\_registration\_plates\_of\_the\_United\_Kingdom](https://en.wikipedia.org/wiki/Vehicle\_registration\_plates\_of\_the\_United\_Kingdom) (didn't use this source too much, but is a great summary of the topic)
