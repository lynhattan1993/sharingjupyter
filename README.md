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
        
        dict_write = dict(zip(list_ma_sv,list_diem_class))
        
    with open(filename_write, 'w') as fileobjwrite:
        for keys,values in dict_write.items():
            fileobjwrite.write(keys + ',' + str(values) + '\n')
            
Analyzing_Report(filename)
```
- Here is a sample running of the program for class1
```
Enter a class to grade (i.e. class1 for class1.txt): class1
Successfully opened class1.txt
**** ANALYZING ****
No errors found!
**** REPORT ****
Total valid lines of data: 20
Total invalid lines of data: 0 
Mean (average) score: 75.55
Highest score: 91
Lowest score: 59
Range of scores: 32
Median score: 73.0
```
- Here is a sample running of the program for class2
```
Enter a class file to grade: class2.txt
Succeessfully opened  class2.txt

**** ANALYZING ****

Invalid line of data: does not contain exactly 26 values:
N00000023,,A,D,D,C,B,D,A,C,C,,C,,B,A,C,B,D,A,C,A,A

Invalid line of data: N# is invalid
N0000002,B,A,D,D,C,B,D,A,C,D,D,D,A,,A,C,D,,A,C,A,A,B,D,D

Invalid line of data: N# is invalid
NA0000027,B,A,D,D,,B,,A,C,B,D,B,A,,A,C,B,D,A,,A,A,B,D,D

Invalid line of data: does not contain exactly 26 values:
N00000035,B,A,D,D,B,B,,A,C,,D,B,A,B,A,A,B,D,A,C,A,C,B,D,D,A,A

**** REPORT ****

Total valid lines of data: 21

Total invalid lines of data: 4

Mean (average) score: 77.9

Highest score: 100

Lowest score: 66

Range of scores: 34

Median score: 76
```

3. Finally, generate a “results” file that contains the detailed results for each student in your class. Each line of this file should contain the student’s ID number, a comma, and then their grade. You should name this file based on the original filename supplied—for example, if the user wants to analyze “class1.txt” you should store the results in a file named “class1_grades.txt”.
```
    dict_write = dict(zip(list_ma_sv,list_diem_class))
        
    with open(filename_write, 'w') as fileobjwrite:
        for keys,values in dict_write.items():
            fileobjwrite.write(keys + ',' + str(values) + '\n')
```            
- Here is a sample running of your program for the first two data files.
```
# this is what class1_grades.txt should look like                               
N00000001,59
N00000002,70
N00000003,84
N00000004,73
N00000005,83
N00000006,66
N00000007,88
N00000008,67
N00000009,86
N00000010,73
N00000011,86
N00000012,73
N00000013,73
N00000014,78
N00000015,72
N00000016,91
N00000017,66
N00000018,78
N00000019,78
N00000020,68
# this is what class2_grades.txt should look like
N00000021,68
N00000022,76
N00000024,73
N00000026,72
N00000028,73
N00000029,87
N00000030,82
N00000031,76
N00000032,87
N00000033,77
N00000034,69
N00000036,77
N00000037,75
N00000038,73
N00000039,66
N00000040,73
N00000041,91
N00000042,100
N00000043,86
N00000044,90
N00000045,67
```
