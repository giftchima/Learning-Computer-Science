


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

    * When a program or script pretends to be a browser and retrieves web pages, looks at those web pages, extracts information, and then looks at more web pages.
    * Search engines scrape web pages - we all this "spidering the web" or "web crawling" 
    


   
 
 
