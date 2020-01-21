


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

#### 1. Which of the following Python data structures is most similar to the value returned in this line of Python:

x = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')

##### Answer: file handle 

#### 2. In this Python code, which line actually reads the data?

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

##### Answer: mysock.recv()

#### 3. Which of the following regular expressions would extract the URL from this line of HTML:
Please click <a href="http://www.dr-chuck.com">  here</a></p>

##### Answer: http://.*

#### 4. In this Python code, which line is most like the open() call to read a file:

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

##### Answer: mysock.connect()

#### 5. Which HTTP header tells the browser the kind of document that is being returned?

##### Answer: Content-Type

#### 6. What should you check before scraping a web site?

##### Answer: That the web site allows scraping

#### 7. What is the purpose of the BeautifulSoup Python library?

##### Answer: It repairs and parses HTML to make it easier for a program to understand.

#### 8. What ends up in the "x" variable in the following code:
html = urllib.request.urlopen(url).read()
soup = BeautifulSoup(html, 'html.parser')
x = soup('a')

##### Answer: A list of all the anchor tags (<a..) in the HTML from the URL

#### 9. What is the most common Unicode encoding when moving data between systems? 

##### Answer: UTF-8

#### 10. What is the ASCII character that is associated with the decimal value 42?

##### Answer: *

#### 11. What word does the following sequence of numbers represent in ASCII: 108, 105, 110, 101
##### Answer: line
#### 12. How are strings stored internally in Python 3?

##### Answer: Unicode

#### 13. When reading data across the network (i.e. from a URL) in Python 3, what method must be used to convert it to the internal format used by strings?

##### Answer: Encode 

## Assignment 1: Scraping HTML Data with BeautifulSoup

In this assignment you will write a Python program to use urllib to read the HTML from the data files below, and parse the data, extracting numbers and compute the sum of the numbers in the file.

#### Data Format
The file is a table of names and comment counts. You can ignore most of the data in the file except for lines like the following:

    <tr><td>Modu</td><td><span class="comments">90</span></td></tr>
    <tr><td>Kenzie</td><td><span class="comments">88</span></td></tr>
    <tr><td>Hubert</td><td><span class="comments">87</span></td></tr>
    
You are to find all the <span> tags in the file and pull out the numbers from the tag and sum the numbers.
Look at the sample code provided. It shows how to find all of a certain kind of tag, loop through the tags and extract the various aspects of the tags.

...

tags = soup('a')  # Retrieve all of the anchor tags
for tag in tags:  # Look at the parts of a tag
   
   print 'TAG:',tag
   print 'URL:',tag.get('href', None)
   print 'Contents:',tag.contents[0]
   print 'Attrs:',tag.attrs
   
You need to adjust this code to look for span tags and pull out the text content of the span tag, convert them to integers and add them up to complete the assignment.

### Answer

    from urllib import request
    from bs4 import BeautifulSoup
    html=request.urlopen('http://python-data.dr-chuck.net/comments_60347.html').read()
    soup = BeautifulSoup(html)
    tags=soup('span')
    sum=0
    for tag in tags:
        sum=sum+int(tag.contents[0])
    print(sum)
    
## Assignment 2: Following Links in HTML Using BeautifulSoup

In this assignment you will write a Python program that expands on https://www.py4e.com/code3/urllinks.py. The program will use urllib to read the HTML from the data files below, extract the href= vaues from the anchor tags, scan for a tag that is in a particular position from the top and follow that link, repeat the process a number of times, and report the last name you find.

Strategy
The web pages tweak the height between the links and hide the page after a few seconds to make it difficult for you to do the assignment without writing a Python program. But frankly with a little effort and patience you can overcome these attempts to make it a little harder to complete the assignment without writing a Python program. But that is not the point. The point is to write a clever Python program to solve the program.

### Answer

    ctx = ssl.create_default_context()    # Ignore SSL certificate errors
    ctx.check_hostname = False
    ctx.verify_mode = ssl.CERT_NONE 

    import urllib.request, urllib.parse, urllib.error
    from bs4 import BeautifulSoup
    import ssl

    taglist=list()
    url = input('Enter URL: ')        # http://py4e-data.dr-chuck.net/known_by_Modu.html

    count=int(input("Enter count:"))
    position=int(input("Enter position:"))
    for i in range(count):
        print ("Retrieving:",url)
        html = urllib.request.urlopen(url, context=ctx).read()
        soup=BeautifulSoup(html,'html.parser')
        tags=soup('a')
        for tag in tags:
            taglist.append(tag)
        url = taglist[position-1].get('href', None)
        del taglist[:]
    print ("Retrieving:",url)




   
    


   
 
 
