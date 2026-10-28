```
print("Unable to open the csv file. Exception:
{}".format(file_read_error))

if __name__ == '__main__':
    read_csv('market_cap.csv')
```

Let’s try to understand what you just implemented in the preceding snippet of code.

After importing the csv module, to keep the code modular, you created a new method named read_csv() that takes in a single parameter – the filename to read the data from:

```
try:
    with open(filename, newline='') as csv_file:
```

Now, if you are not familiar with the approach of opening the file shown in the preceding snippet, this is also known as the try-with-resources approach. In this case, any block of code that is encapsulated in the scope of the with block will have access to the file object, and once the code exits the scope of the with block, the file will be closed automatically.

# Important note

It is a good habit to encapsulate file I/O operations within a try-except block since file I/O can fail for several reasons and showing stack traces to the users is not a good option.

The reader() method returns a reader object over which you can iterate to access the values, as we saw in the Reading data from a CSV file section:

```
for record in csv_reader:
    print(record)
```

Once this is done, you must write the entry point method, from which your code will begin executing, by calling the read_csv() method and passing the name of the CSV file to read:

```
if __name__ == '__main__':
    read_csv('market_cap.csv')
```

4. With this, you are done and ready to parse your CSV file. You can do this by running your Python file in a Terminal or Command Prompt, as shown here:

```
python3 csv_reader.py
```