## Finding Numbers in a Haystack


*In this assignment you will read through and parse a file with text and numbers. You will extract all the numbers in the file and compute the sum of the numbers.*

**Data Files**

We provide two files for this assignment. One is a sample file where we give you the sum for your testing and the other is the actual data you need to process for the assignment.

**Data Format**

*The file contains much of the text from the introduction of the textbook except that random numbers are inserted throughout the text.*

**Handling The Data**

*The basic outline of this problem is to read the file, look for integers using the re.findall(), looking for a regular expression of '[0-9]+' and then converting the extracted strings to integers and summing up the integers.*

**Answer:**

### code
import re

hand = open("regex_sum_60345.txt")
x=list()
for line in hand:
     y = re.findall('[0-9]+',line)
     x = x+y

sum=0
for z in x:
    sum = sum + int(z)

print(sum)

Sum = 369400




