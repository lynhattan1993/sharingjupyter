# Project title: GRADES THE EXAMS
## Introduction

I will write a program to:
1. Open requested external text files with exception-handling
2. Scan each line of the exam answers for valid data and provide a corresponding report
3. Grade each exam based on the rubric provided and reportGrade each exam based on the rubric provided and report
4. Generate an appropriately-named result file

## Usage
1. Open file
- Create a new Python program called “lastname_firstname_grade_the_exams.py.” (Make sure your source code file is inside of the same folder as the data files you just downloaded.)
- Next, write a program that lets the user type in the name of a file. Attempt to open the supplied file for reading access. If the file exists you can print out a confirmation message. If the file doesn’t exist you should tell the user that the file cannot be found and prompt them again.
- Use a try/except block to do this:

`filename = input('Enter a class file to grade: ')`
```try:
    with open(filename) as fileobj:
        print('Succeessfully opened  ' + filename + '\n')
 except:
    print('File cannot be found')```
