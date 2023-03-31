    import os import time import chardet

These lines import necessary libraries - os for file and directory operations, time for measuring execution time and chardet for detecting file encoding.

    start_time = time.time() directory_path = "" keywords = [] num_files = 0

This block initializes the start time, sets the path to the directory to search for files, defines keywords to search for, and initializes a counter for the number of files containing the keywords.

    for dirpath, dirnames, filenames in os.walk(directory_path): for file_name in filenames: file_path = os.path.join(dirpath, file_name) with open(file_path,     'rb') as f: contents = f.read() detected_encoding = chardet.detect(contents)['encoding']

This block uses the os.walk() function to recursively traverse the directory and subdirectories to get all filenames. For each file, the full path is obtained using os.path.join(). The file is then opened in binary mode and its contents are read. The encoding of the file is detected using chardet and stored in the detected_encoding variable.

    encodings_to_try = [detected_encoding, 'utf-8', 'iso-8859-1'] for encoding in encodings_to_try: try: with open(file_path, 'r', encoding=encoding) as f:       contents = f.read() for keyword in keywords: if keyword in contents: print('Found keyword in file:', file_path) print('Keyword:', keyword) num_files += 1     break break except UnicodeDecodeError: continue

This block creates a list of encodings to try in order of priority, and then attempts to open the file with each encoding until one succeeds or all fail. If the file can be opened, its contents are read and searched for each of the specified keywords.

    with open('output.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['Keyword', 'File Name', 'File Path'])
    for keyword, files in keyword_files.items():
        if files:
            writer.writerow([keyword])
            for file_name, file_path in files:
                writer.writerow(['', file_name, file_path])

The code opens a new file called output.csv in write mode using the csv.writer() function. It writes the header row with the column names 'Keyword', 'File Name', and 'File Path'. The code then loops through each keyword and its associated list of files using the keyword_files dictionary. 

    if all(len(files) == 0 for files in keyword_files.values()):
    print('None of the keywords were found in any file.')
    else:
    with open('output.csv', 'r') as f:
        print(f.read())
The code checks if any of the lists of files associated with the keywords are empty. If all of the lists are empty, it prints a message indicating that none of the keywords were found in any file. Otherwise, it opens the output.csv file again in read mode and prints its contents to the console.

    end_time = time.time()
    print('Total number of files searched:', sum([len(files) for r, d, files in os.walk(directory_path)]))
    print('Total execution time:', end_time - start_time, 'seconds')
Finally, the code stops the timer and prints the total number of files searched (by counting the number of files returned by the os.walk() function) and the total execution time in seconds.





![archit](https://user-images.githubusercontent.com/128144427/229096616-4387eef8-4451-49b9-af8c-ae58aecd520c.jpg)


