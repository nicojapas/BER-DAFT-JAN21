# Lab | Bash

## Exercises solutions

- Print in console "Hello World".

   - **Solution:**`echo "Hello World"`

- Create a directory called `new_dir`.

    - **Solution:**`mkdir new_dir`

- Remove that directory.

    - **Solution:**`rm -rf new_dir`

- Copy the file `sed.txt` from `lorem` folder to `lorem-copy` folder.

    - **Solution:**`cp lorem/sed.txt lorem-copy/sed.txt`

- Copy the other two files `at.txt` and `lorem.txt` from lorem folder to lorem-copy folder in just one line using a semicolon `;`.

   - **Solution:** cp lorem/at.txt lorem-copy/; cp lorem/lorem.txt lorem-copy/

- Show the `sed.txt` file content from `lorem` folder.

    - **Solution:**`cat lorem/sed.txt`

- Show the `at.txt` file and `lorem.txt` file contents from `lorem` folder.

    - **Solution:**`cat lorem/at.txt; cat lorem/lorem.txt`

- Print the first 3 rows in `sed.txt` file from `lorem-copy` folder.

    - **Solution:**`head -3 lorem-copy/sed.txt`

- Print the last 3 rows in `sed.txt` file from `lorem-copy` folder.

    - **Solution:**`tail -3 lorem-copy/sed.txt`

- Add `Homo homini lupus.` at the end of `sed.txt` file from `lorem-copy` folder

    - **Solution:**`echo "Homo homini lupus." >> lorem-copy/sed.txt`

- Print the last 3 rows in `sed.txt` file from `lorem-copy` folder. You should see `Homo homini lupus.`.

    - **Solution:**`tail -3 lorem-copy/sed.txt`

- sed command is used to replace the text in a file. Use the sed command to replace all occurances of et with ET in the file at.txt file present in the folder lorem.

      **Solution**:
      sed -i -r 's/et/ET/g' sed.txt;
      cat sed.txt

- Find who is the system user.

    - **Solution:**`whoami`

- Find which is your actual path.

    - **Solution:**`pwd`

- List all files with the extension `.txt` in lorem folder.

    - **Solution:**`ls lorem/*.txt`


## Bonus Solutions

- Store your `name` in a variable with `read` command.

    - **Solution:**`NAME="your_name"`

- Print that variable.

    - **Solution:**`echo $NAME`

- Create a new directory named with variable `name`.

    - **Solution:**`mkdir $NAME`

- Remove that directory.

    - **Solution:**`rm -rf $NAME`
