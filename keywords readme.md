  import os import time import chardet

These lines import necessary libraries - os for file and directory operations, time for measuring execution time and chardet for detecting file encoding.

  start_time = time.time() directory_path = "" keywords = [] num_files = 0

This block initializes the start time, sets the path to the directory to search for files, defines keywords to search for, and initializes a counter for the number of files containing the keywords.

  for dirpath, dirnames, filenames in os.walk(directory_path): for file_name in filenames: file_path = os.path.join(dirpath, file_name) with open(file_path,     'rb') as f: contents = f.read() detected_encoding = chardet.detect(contents)['encoding']

This block uses the os.walk() function to recursively traverse the directory and subdirectories to get all filenames. For each file, the full path is obtained using os.path.join(). The file is then opened in binary mode and its contents are read. The encoding of the file is detected using chardet and stored in the detected_encoding variable.

  encodings_to_try = [detected_encoding, 'utf-8', 'iso-8859-1'] for encoding in encodings_to_try: try: with open(file_path, 'r', encoding=encoding) as f:       contents = f.read() for keyword in keywords: if keyword in contents: print('Found keyword in file:', file_path) print('Keyword:', keyword) num_files += 1     break break except UnicodeDecodeError: continue

This block creates a list of encodings to try in order of priority, and then attempts to open the file with each encoding until one succeeds or all fail. If the file can be opened, its contents are read and searched for each of the specified keywords. If a keyword is found, the file path and keyword are printed, the counter for the number of files containing keywords is incremented, and the search for this file is stopped. If no encoding can successfully decode the file, the program moves on to the next file.

  if num_files == 0: print('Keyword not found in any file') end_time = time.time() print('Number of files searched:', sum([len(files) for r, d, files in         os.walk(directory_path)])) print('Number of files with keyword:', num_files) print('Execution time:', end_time - start_time)

This block checks if any files were found with the specified keywords and prints a message accordingly. It then calculates the total number of files searched and the execution time of the program. Finally, it prints the number of files containing the keywords and the execution time.

Markdown 2817 bytes 372 words 56 lines Ln 48, Col 0
