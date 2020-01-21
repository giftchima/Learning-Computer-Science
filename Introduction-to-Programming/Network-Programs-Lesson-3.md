


## Unicode Characters and Strings

### Multi-Byte Characters
1. To represent the wide range of characters computers must handle, we represent characters with more than one byte.
   * UTF-16 - Fixed length - Two bytes
   * UTF-32 - Fixed length - Four bytes
   * UTF-8 - 1-4 bytes
   * Upwards compatible with ASCII
   * Automatic detection between ASCII and UTF-8
   * UTF-8 is recommended practice for encoding data to be exchanged between systems.
   
 2. In Python 3, all strings interally are UNICODE. When we talk to a network resource using sockets or talk to a database we have to encode and decode (usually to UTF-8)
 
 3. Encode() function in python takes input and converts it to bytes. 
 4. Decode() function in python takes and converts it from bytes to Unicode. 
 
 (ADD IMAGE HERE)
 
 ### Retrieving Web Pages
 
 #### Using urllib in Python
 
 Sine HTTP is so common, we have a library that does all the socket work for us and makes web pages look like a file.
 
    import urllib.request, urllib.parse, urllib.error
    fhand = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')
    for line in fhand:
        print(line.decode().strip())
        
#### another example
  
    import socket

    # This is using HTTP 1.0 - not all servers support the oldest protocol
    # Try http://data.pr4e.org/romeo.txt if your server fails.

    url = input('Enter: ')
    words = url.split('/')
    host = words[2]

    mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    mysock.connect((host, 80))
    mysock.send(('GET '+url+' HTTP/1.0\r\n\r\n').encode())

    while True:
        data = mysock.recv(512)
        if (len(data) < 1):
            break
        print(data.decode(), end='')

    mysock.close()
    
    
#### another example

    import urllib.request, urllib.parse, urllib.error

    fhand = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')

    counts = dict()
    for line in fhand:
        words = line.decode().split()
        for word in words:
            counts[word] = counts.get(word, 0) + 1
    print(counts)
    
### Parsing Web Pages
 
#### What is Web Scraping?

1. Why Scrape? 
   * Pull data - particularly social data - who link to who?
   * Get your own data back out of some system that has no "export capability"  
   * Monitor a site for new information
   * Spider the web to make a database for a search engine
   * When a program or script pretends to be a browser and retrieves web pages, looks at those web pages, extracts information, and then looks at more web pages.
   * Search engines scrape web pages - we all this "spidering the web" or "web crawling" 
   * To parse HTML, you can used a free library called BeautifulSoup from www.crummy.com
   
   #### code example
   
       # To run this, download the BeautifulSoup zip file
       # http://www.py4e.com/code3/bs4.zip
       # and unzip it in the same directory as this file

       import urllib.request, urllib.parse, urllib.error
       from bs4 import BeautifulSoup
       import ssl

       # Ignore SSL certificate errors
       ctx = ssl.create_default_context()
       ctx.check_hostname = False
       ctx.verify_mode = ssl.CERT_NONE

       url = input('Enter - ')
       html = urllib.request.urlopen(url, context=ctx).read()
       soup = BeautifulSoup(html, 'html.parser')

       # Retrieve all of the anchor tags
       tags = soup('a')
       for tag in tags:
           print(tag.get('href', None))

## Quiz: Reading Web Data From Python

1. Which of the following Python data structures is most similar to the value returned in this line of Python:

x = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')

** Answer: file handle **

2. In this Python code, which line actually reads the data?

import socket

mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
mysock.connect(('data.pr4e.org', 80))
cmd = 'GET http://data.pr4e.org/romeo.txt HTTP/1.0\n\n'.encode()
mysock.send(cmd)

while True:
    data = mysock.recv(512)
    if (len(data) < 1):
        break
    print(data.decode())
mysock.close()

3. Which of the following regular expressions would extract the URL from this line of HTML:
<p>Please click <a href="http://www.dr-chuck.com">here</a></p>

4. In this Python code, which line is most like the open() call to read a file:

import socket

mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
mysock.connect(('data.pr4e.org', 80))
cmd = 'GET http://data.pr4e.org/romeo.txt HTTP/1.0\n\n'.encode()
mysock.send(cmd)

while True:
    data = mysock.recv(512)
    if (len(data) < 1):
        break
    print(data.decode())
mysock.close()

5. Which HTTP header tells the browser the kind of document that is being returned?

6. What should you check before scraping a web site?

7. What is the purpose of the BeautifulSoup Python library?

8. What ends up in the "x" variable in the following code:
html = urllib.request.urlopen(url).read()
soup = BeautifulSoup(html, 'html.parser')
x = soup('a')

9. What is the most common Unicode encoding when moving data between systems?


   
    


   
 
 
