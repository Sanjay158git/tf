        import os import time import chardet import csv import requests

This section is importing necessary modules such as os for operating system functionality, time for timing the execution of the code, chardet for detecting the encoding of a file, csv for writing the output data to a CSV file, and requests for making HTTP requests to the GitHub API.

        start_time = time.time()

This line sets a variable start_time to the current time using the time.time() function. This will be used later to calculate the total execution time of the code.

        repo_owner = 'NAME' repo_name = 'NAME

These lines set the variables repo_owner and repo_name to the owner and name of the GitHub repository to search.

        keywords = ['KEYWORDS]

This line sets a list of keywords to search for in the files of the repository. In this case, the keywords are function, class, break, and def.

        keyword_files = {keyword: [] for keyword in keywords}

This line creates an empty dictionary called keyword_files, where each keyword in the keywords list will be a key and the value will be an empty list to store the file paths where the keyword is found.

        retrieve contents of repository from GitHub API api_url = f'https://api.github.com/repos/{repo_owner}/{repo_name}/contents' headers = {'Accept':            'application/vnd.github.v3+json'} response = requests.get(api_url, headers=headers) response.raise_for_status() contents = response.json()

These lines use the requests.get() function to retrieve the contents of the specified GitHub repository using the GitHub API. The API URL is constructed using the repo_owner and repo_name variables, and the headers variable sets the API version to v3. The response.raise_for_status() line will raise an exception if the request fails, and response.json() returns the JSON response as a Python dictionary, which is assigned to the contents variable.

        if content['type'] =  'dir':
    subfolder_url = content['url']
    response = requests.get(subfolder_url, headers=headers)
    response.raise_for_status()
    subfolder_contents = response.json()
    for subcontent in subfolder_contents:
        if subcontent['type'] == 'file':
            response = requests.get(subcontent['download_url'])
           response.encoding = chardet.detect(response.content)['encoding']
            contents = response.text
            for keyword in keywords:
                if keyword in contents:
                    keyword_files[keyword].append(subcontent['path'])
This section finds the keword inside the repositories.

       elif content['type'] == 'file':
    response = requests.get(content['download_url'])
    response.encoding = chardet.detect(response.content)['encoding']
    contents = response.text
    for keyword in keywords:
        if keyword in contents:
            keyword_files[keyword].append(content['path'])` 
This section loops through each item in the contents dictionary retrieved from the GitHub API. If the item is a directory, it retrieves the contents of the directory using the API and loops through each file in the directory, retrieving the file’s contents using the API and detecting the file’s encoding using chardet.detect(). Then, it checks if each keyword is in the file contents and it oprints in csv file.

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


![archit (1)](https://user-images.githubusercontent.com/128144427/229495698-d48b68bf-8577-44cf-b3ed-5fcaeab60d33.jpg)




