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
```
filename = input('Enter a class file to grade: ')
try:
  with open(filename) as fileobj:
    print('Succeessfully opened  ' + filename + '\n')
except:
  print('File cannot be found')
```
- Here is a sample running of the program
```
Enter a class file to grade (i.e. class1 for class1.txt): foobar
File cannot be found.
Enter a class file to grade (i.e. class1 for class1.txt): class1
Successfully opened class1.txt
```
2. Write a function: Ananlyzing_Report to
- Report the total # of lines of data stored in the file.
- Analyze each line and make sure that it is “valid.”
  -  A valid line contains a comma-separated list of 26 values
  -  The N# for a student is the first item on the line. It should contain the character “N” followed by 8 numeric characters.
- If a line of data is not valid you should report it to the user by printing out an error message. You should also count the total # of valid lines of data in the file.
- Calculate following statistics for the entire class:
  -  The average score
  -  The highest score
  -  The lowest score
  -  The range of scores (the highest minus the lowest)
  -  The median value
```
def Analyzing_Report(filename):
    with open(filename,'r') as fileobj:
        total_valid_lines = 0
        total_invalid_lines = 0
        total_lines = 0
        list_diem_class = []
        list_ma_sv = []
        
        print('**** ANALYZING ****' + '\n')
        for line in fileobj:
            total_lines += 1
            line.rstrip()
            list_line = line.split(',')
            match = re.fullmatch(r'N[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]',list_line[0])
            
            answer_key = "B,A,D,D,C,B,D,A,C,C,D,B,A,B,A,C,B,D,A,C,A,A,B,D,D"
            list_answer_key = answer_key.split(',')
            
            if len(list_line) == 26 and match:             
                total_valid_lines += 1
                list_line_sub = list_line[1:]
                list_line_sub[-1] = list_line_sub[-1][0]
        
                list_ma_sv.append(list_line[0])
                
                diem_tung_nguoi = 0
                
                for i in range(0,len(list_line_sub)):
                   if list_line_sub[i] == list_answer_key[i]:
                      diem_tung_nguoi += 4
                   elif list_line_sub[i] == '':
                      diem_tung_nguoi += 0
                   elif list_line_sub[i] != list_answer_key[i]:
                      diem_tung_nguoi -= 1    

                list_diem_class.append(diem_tung_nguoi)
                
            elif len(list_line) != 26:
                total_invalid_lines += 1
                print('Invalid line of data: does not contain exactly 26 values:')
                print(line)
            elif not(match):
                total_invalid_lines += 1
                print('Invalid line of data: N# is invalid')
                print(line)
        
        if total_lines == total_valid_lines:
            print('No error found!' + '\n')
            
        print('**** REPORT ****' + '\n')        
        print('Total valid lines of data: ' + str(total_valid_lines) + '\n')
        print('Total invalid lines of data: ' + str(total_invalid_lines) + '\n')

        average_score = round((sum(list_diem_class)/ len(list_diem_class)),2)
        max_score = max(list_diem_class)
        min_score = min(list_diem_class)
        range_score = max_score - min_score

        list_diem_class_sort = sorted(list_diem_class)
        leng = len(list_diem_class_sort)
        if leng % 2 == 0:
            median = (list_diem_class_sort[((leng//2) - 1)] + list_diem_class_sort[(leng//2)]) / 2
        else:
            median = list_diem_class_sort[(leng//2)]

        print('Mean (average) score: ' + str(average_score) + '\n')
        print('Highest score: ' + str(max_score) + '\n')
        print('Lowest score: ' + str(min_score) + '\n')
        print('Range of scores: ' + str(range_score) + '\n')
        print('Median score: ' + str(median))

        filename_write = filename[0:6] + '_grades.txt'

        print(list_diem_class)
        print(list_ma_sv)

        dict_write = dict(zip(list_ma_sv,list_diem_class))
        print(dict_write)
        
    with open(filename_write, 'w') as fileobjwrite:
        for keys,values in dict_write.items():
            fileobjwrite.write(keys + ',' + str(values) + '\n')
            
Analyzing_Report(filename)
```
