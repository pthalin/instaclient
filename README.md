# instaclient

**instaclient** is a Python library for accessing Instagram's features.
With this library you can create Instagram Bots with ease and simplicity. The InstaClient takes advantage of the selenium library to excecute tasks which are not allowed in the Instagram Graph API (such as sending DMs).

The only thing you need to worry about is to spread your requests throughout the day to avoid reaching Instagram spam limits.
> **Disclaimer:** Please note that this is a research project. **I am by no means responsible for any usage of this tool.** Use it on your behalf. I'm also not responsible if your accounts get banned due to the extensive use of this tool.

## Table of Contents

1. [Current Features](#current-features)
2. [Installation](#installation)
3. [Usage](#usage)
4. [Contributing](#contributing)
5. [Help - Community](#help-community)
6. [Credits](#credits)
7. [License](#license)

## Current Features
- Follow a user
- Unfollow a user
- Search posts by tag
- Send DMs to users
- Like a users latest posts (max 15 for now - not working yet)
- Get a users image media (not working yet)

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install instaclient.

```bash
pip install instaclient
```
To update the package:
```bash
pip install -U instaclient
```

## Usage
#### CREATE THE CLIENT
```python
from instaclient import InstaClient
from instaclient.errors import *

# Create a instaclient object
client = Instaclient(driver=InstaClient.CHROMEDRIVER) # Only the ChromeDriver is available at the moment
```
You can also directly insert your account's username and password when initiating the client:
```python
from instaclient import InstaClient
from instaclient.errors import *

# Create a instaclient object
client = Instaclient(username='username', password='password') 
# You can omit specifying the driver as it will fall back to the ChromeDriver
```
#### LOGIN INTO INSTAGRAM
```python
from instaclient.errors import *

try:
    client.login(username=username, password=password) # Go through Login Procedure
except SecurityCodeNecessary:
    code = input('Enter the 2FA security code generated by your Authenticator App or sent to you by SMS')
    try:
        client.input_security_code(code)
    except InvalidSecurityCodeError:
        # If entered code is incorrect:
        print('The code is incorrect')
```
#### SEND A DIRECT MESSAGE
```python
result = client.send_dm('username', 'Message to send') # send a DM to a user
```
> Make sure to distrubute your client.send_dm() requests over a period of time to avoid reaching Instagram's spam limits.
#### GET A USER'S FOLLOWERS
```python
followers = client.scrape_followers(user='username', count=100, callback_frequency=10)
# Get first 100 followers of the user, returning a callback every 10 followers (default callback is a terminal print statement)
```
> The client.scrape_followers() method can take a lot of time depending on the amount of followers you want to scrape. It is advised not to overceed 1000 followers, as the execution time rises exponentially. If you must query a large amount of followers, make sure to run the method in a worker.

This method might be updated in the near future to cache scraped data in a SQLite database or to scrape the followers in a separate thread with a queue.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
Please make sure to update [tests](https://github.com/wickerdevs/instaclient/tree/master/tests) as appropriate.

## Help Community
You can join this [Telegram Group](https://t.me/instaclient) to ask questions about the instabot's functionalities or to contribute to the package!

## Credits
[AUTHORS](https://github.com/wickerdevs/instaclient/blob/master/AUTHORS.rst)

## License
[MIT](https://choosealicense.com/licenses/mit/)

