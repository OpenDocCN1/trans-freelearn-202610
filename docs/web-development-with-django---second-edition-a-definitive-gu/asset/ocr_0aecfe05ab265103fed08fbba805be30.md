# Important note

While the preceding steps outline the verbose version of writing to a CSV file to provide better clarity of the underlying working of how CSV files should be structured, we could have merged the step of writing the header and data into a single line of code by having the header list as the first element of the data list and calling the writerows() method with the data list as a parameter.

2. Once we have created the methods that can write the provided data to a CSV file, we must write the code for the entry point call, and inside it, set up the values for the header, data, and filename fields, and finally call the write_csv() method that we defined earlier:

```
if __name__ == '__main__':
    header = ['name', 'age', 'gender']
    data = [['Richard', 32, 'M'], ['Mumzil', 21, 'F'],
            ['Melinda', 25, 'F']]
    filename = 'sample_output.csv'
    write_csv(filename, header, data)
```

3. Now, with the code in place, execute the file we just created and see whether it creates the CSV file. To execute it, run the following command:

```
python3 csv_writer.py
```

Once the execution finishes, you will see that a new file has been created in the same directory as the one in which you executed the command. When you open the file, the contents should resemble what you can see in the following figure:

![](bbox=[0.25, 0.57, 0.72, 0.88])

Figure 13.3: Output from the CSV writer sample_output.csv