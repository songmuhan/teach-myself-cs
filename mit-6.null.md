# The Missing Semester of Your CS Education

- progress
	- [x] Course Overview + the shell
	- [ ] Shell Tools and Scripting
	- [ ] Editors (Vim)
	- [ ] Command-line Environment
	- [ ] Version Control (Git)
	- [ ] Debugging and Profiling
	- [ ] Meta-programming
	- [ ] Security and Cryptography
	- [ ] Potpourri

## Course Overview + the Shell

- course overview

- shell

  - what is shell ?

  - Using the shell

    - program: such as **date**

    - argument:  

      - program name + whitespace + argument

      	- ```shell
      		$ echo hello
      		```

      - whitespace in arguments?

      	- ```shell
      		$ echo "hello world"
      		$ echo 'hello world'
      		$ echo hello\ world
      		```

      	- ==diff of "" and ''?==

      		- single quotes won't interpolate anything, but double quotes will.

      			- single quotes

      				> Enclosing characters in single quotes (`'`) preserves the literal value of each character within the quotes. A single quote may not occur between single quotes, even when preceded by a backslash.

      			- double quotes

      				> Enclosing characters in double quotes (`"`) preserves the literal value of all characters within the quotes, with the exception of $,\`,\, and, when history expansion is enabled, !.

      			

    - how does the shell know where to find the **date** or **echo** programs?

    	- environment variables called **$PATH**

    	- ```sh
    		$ echo $PATH
    		$ which echo
    		$ where echo
    		$ /bin/echo $PATH
    		```

    		- specify what program you want to run with **path**

  - Navigating in the shell

    - **path** : a delimited list of directories, separated by / or \

      - for linux or MacOs : / is the root of the file system, under which all directories and files lie
      - absolute path and relative path
      - ==how does the shell search the path efficiently ?==
      	- it seems to just loop the list of all environment variables, and picks the first match one

    - programs

    	- pwd 

    	- cd

    		- . represent for the current directory and .. represent for its parent directory

    	- ls

    		- argument:  

    			```shell
    			# -l use a long listing format
    			missing:~ $ ls -l /home
    			drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
    			```

    			- d -> directory
    			- rwx -> permission
    			- \- the given principal does not have the 

    	- mv, cp, mkdir

  - connecting programs

  	- < file and > file: change the input and output stream

  		```shell
  		$ echo hello > hello.txt
  		$ cat < hello.txt
  		$ cat < hello.txt > hello2.txt
  		```

  	- \>> to append to a file

  		- **bad**: 

  			```shell
  			$ cat hello.txt >> hello.txt
  			```

  	- **pipes**:

  		```shell
  		$ ls -l / | tail -n1
  		```

  - A versatile and powerful tool

  	- sudo
  	- |, >, < are done by shell, not individual program

- Exercises

  - ```shell
  	$ mkdir tmp
  	$ mkdir tmp/missing
  	$ cd tmp/missing
  	$ touch semester 
  	$ echo '#!/bin/sh' > semester   ### note: single quote
  	$ echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
  	```

  - ```shell
  $ ./semester
  	-rw-r--r--@ 1 songmuhan  staff  61 Mar  2 15:53 semester
  	```
  	
  	- the semester file are not executable without autorization
  	
  - ```shell
  	$ sh semester
  	$ chmod u+x semseter
  	$./semester
  	$stat -x semester | grep -iF modify > last-modified.txt
  	```

  	

  

  ​	

## Shell Tools and Scripting

### Shell Scripting

- scripting language

  - variables

    ```shell
    $ foo=bar
    $ echo "$foo"
    bar
    $ echo '$foo'
    $foo
    ```

  - function 

  	```shell
  	mcd(){
  		mkdir -p "$1"
  		cd "$1"
  	}
  	```

  - exit code

  	- 0 -> everything went OK, else an error occurred

  	```shell
  	false || echo "Oops, fail"
  	# Oops, fail
  	
  	true || echo "Will not be printed"
  	#
  	
  	true && echo "Things went well"
  	# Things went well
  	
  	false && echo "Will not be printed"
  	#
  	
  	true ; echo "This will always run"
  	# This will always run
  	
  	false ; echo "This will always run"
  	# This will always run
  	
  	```

  	- ; is treated an end of command character

  - ```shell
    #!/bin/bash
    echo "Starting program at $(date)"
    echo "Running program $0 with $# arguments with pid $$"
    for file in "$@"; do
    	grep foobar "$file" > /dev/null 2> /dev/null
    	if [[$? -ne 0]]; then
    		echo "File $file does not have any foobar, adding one"
    		echo "# foobar" >> "$file"
    	fi
    done
    ```

    

