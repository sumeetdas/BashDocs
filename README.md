# BashDocs
Bash documentation

###pwd

`pwd` stands for "print working directory". Prints the absolute path of the current directory. 

__Example__

> # Assuming that we are at root directory
> pwd
> /
> # Change directory to /home
> cd home
> pwd
> /home
> # Change directory to /home/carrot
> cd carrot
> pwd
> /home/carrot

###cd

'cd' stands for "change directory". Changes the directory to the specified one.

__Example__

> # Switch to root directory
> cd /
> # Switch to home directory
> cd ~
> pwd
> /home/<username>
> # Switch to child directory "Downloads"
> cd Downloads
> pwd
> /home/<username>/Downloads
> # Switch to /another/directory
> cd /another/directory
> pwd
> /another/directory

###awk

The 'awk' name was obtained using the first characters of programmers Aho, Weinberger and Kernighan. 

awk is used to search files for lines containing some pattern(s). When a line matches one of the patterns, special actions are performed on that line.

awk programs are "data-driven": you describe the data you want to work with and then what to do when you find it.

Two common ways to run awk:

1. If the program is short, it is easiest to run it on the command line:

> awk PROGRAM inputfile(s)

2. If multiple changes have to be made, possibly regularly and on multiple files, it is easier to put the awk commands in a script (with extension .awk, e.g. FILENAME.awk):

> awk -f PROGRAM-FILE inputfile(s)

When awk reads a line, it splits it into substrings seperated by whitespace (space or tabs). awk stores these substrings into awk variables $1, $2, $3, etc. awk variable $0 stores the entire line.

awk variables are variables defined during the execution of the awk program. The scope of these variables is restricted to the awk program and hence can't be accessed in the bash shell. So, running **echo $0** after running awk script would display nothing, unless you had defined it earlier.

* How do we print awk variables? By using 'print' command.

Suppose we have a file named data.txt containing the names of students and marks obtained by them, seperated by space as follows:

``
Ram 80
Shyam 85
Mohan 75
``

To simply print the data as is, we can run the following

``
> awk '{print $1, $2}' data.txt
``
If you remove the comma between $1 and $2, then the names and marks will not have space in between, since awk will treat $1 and $2 as one argument.

We can insert strings between awk variables as follows:

``
> awk '{print "Marks obtained by " $1 " = " $2}' data.txt
``

* How do we precede output with comments? By using the BEGIN statement:

``
> awk 'BEGIN {print "Printing data...\n"} {print $1}' data.txt
``

* How do we split a line by comma instead of whitespace? By changing the value of pre-defined awk variable **FS (input field seperator)**. Example:

``
awk 'BEGIN { FS="," } { print $1, $2 }' commaSeperatedValues.txt
``

* How do we add / print text after printing the output? By using the END statement:

``
> awk '{print $1, $2} END { print "Printing is done." }' data.txt
``

* How do we split only those line who match some pattern? By using regular expressions.

Suppose we have a file named gtaPlaces.txt containing the following data:

San Andreas
Los Santos
Liberty City
Las Venturas
San Fierro

and we want to process only those lines which start with "San". We can do so with awk as follows:

``
> awk '/^San.*/ {print $1, $2}' gtaPlaces.txt
``

* The output from an entire print statement is called an **output record**. Each print command results in one output record, and then outputs a string called the **output record separator, ORS**. 

You would remember that if we seperate awk variables with comma in print command, the variable values are seperated by a single space. If you would like to instead insert comma, then change the value of OFS (Output Field Seperator) to comma.

Example: The following command will seperate variables with comma and print "===" (without quotes) in a seperate line after each record:

``
> awk 'BEGIN{OFS="," ; ORS="\n===\n"} {print $1, $2}' data.txt
``

NOTE: The default value of ORS is newline (\n). If you override it with just ===, then all records will be printed in one single line seperated by ===. To print === in seperate line, don't forget to prepend and append it with \n (as shown in the example).

* The built-in NR holds the number of records that are processed. It is incremented after reading a new input line. You can use it at the end to count the total number of records, or in each output record:

``
> awk '{print "Line number " NR " : " $1, $2} END {print "Total lines processed: " NR}' gtaPlaces.txt
``

* How do we sum across values in a particular field/column? By using user-defined variables.

Apart from the built-in variables, you can define your own. When awk encounters a reference to a variable
which does not exist (which is not predefined), the variable is created and initialized to a null string. For all subsequent references, the value of the variable is whatever value was assigned last. 

Variables can be a string or a numeric value. Content of input fields can also be assigned to variables.

Example: Suppose we have a file named "marks.txt" containing a student's marks in various subjects:

``
Physics 80
Chemistry 75
Mathematics 85
Computer_Science 90
``

We would like to compute and display the sum of all marks after displaying the data in some format. Using awk, we can implement it as follows:

``
> awk '{total=total + $2} {print "Mohan obtained " $2 " marks in " $1} END {print "Total marks obtained " total}' marks.txt
`` 

