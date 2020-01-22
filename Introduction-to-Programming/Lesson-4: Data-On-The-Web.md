## eXtensible Markup Language (XML)

### XML Basics 
![Image description](XML.png)

- Tags indicate the beginning and ending of elements
- Attibutes - Keyword/value pairs on the opening tag of XML
- Serialize/ De-Serialize - Convert data in one program into a common format that can be stored and/or transmitted between systems in a programming language-independent manner.

## XML Schema
- Description of the legal format of an XML document.
- Expressed in terms of constaints on the structure and content of documents.
- Often used to specify a "contract" between systems - "My system will only accept XML that conforms to this particular Schema."
- If a particular piece of XML meets the specification of the Schema - it is said to "validate"

## Parsing XML
put in xmll.py

## Assignment
Extracting Data from XML
In this assignment you will write a Python program somewhat similar to https://py4e.com/code3/geoxml.py. The program will prompt for a URL, read the XML data from that URL using urllib and then parse and extract the comment counts from the XML data, compute the sum of the numbers in the file and enter the sum.

### Data Format and Approach

You are to look through all the <comment> tags and find the <count> values sum the numbers. The closest sample code that shows how to parse XML is geoxml.py. But since the nesting of the elements in our data is different than the data we are parsing in that sample code you will have to make real changes to the code.
To make the code a little simpler, you can use an XPath selector string to look through the entire tree of XML for any tag named 'count' with the following line of code:
       counts = tree.findall('.//count')
  
Take a look at the Python ElementTree documentation and look for the supported XPath syntax for details. You could also work from the top of the XML down to the comments node and then loop through the child nodes of the comments node.

## Answer


    ctx = ssl.create_default_context()   # Ignore SSL certificate errors
    ctx.check_hostname = False
    ctx.verify_mode = ssl.CERT_NONE

    import urllib
    import xml.etree.ElementTree as ET

    url = input("Enter URL: ")
    if len(url) > 1:
        print ("Retrieving " + url)

    xml = urllib.request.urlopen(url, context=ctx).read()
    print ("Retrieved: " + str(len(xml)) + " characters")

    tree = ET.fromstring(xml)

    counts =  tree.findall('.//count')
    print ("Count: " + str(len(counts)))

    accumulator = 0

    for count in counts:
        accumulator += int(count.text)

    print ("Sum:" + str(accumulator))






