# Example 2 – Open Library book details

Open Library (https://openlibrary.org/developers/api) provides an experimental search API. It has various API-related options for its collection of books and similar content. For this example, https://openlibrary.org/dev/docs/api/search will be used to find book data with the subject python and the author Wes (the result is available via https://openlibrary.org/search.json?q=python&author=Wes).

As defined in the following code, apiUrl searches openlibrary.org for the defined subject and author variables. The response from apiUrl is in JSON format but is nested in structure. The HTTP request to apiUrl is processed along with a few selected HTTP headers. The json() method and requests.get() reveals that the content is in JSON format:

```
subject="python"
author="Wes"
apiUrl="https://openlibrary.org/search.json?q="+subject+"&author="+author

headers={
'Upgrade-Insecure-Requests': '1',
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36'
}
response = requests.get(apiUrl, headers=headers).json()
```

Information about books is provided under various indexes, titles, or keys, found inside response['docs']. Values for fields such as 'book_title', 'published_date', 'publisher', 'author', and 'subjects' are collected (if available) in a list named lists:

```
lists=[]  # Collector
for book in response['docs']:
    if 'subject_key' in book: # check if subject_key exists
        lists.append([book['title'],
        book['publish_date'][0],
        book['publisher'][0],
        book['author_name'][0],
        "|".join(book['subject_key'])])
    else:
        lists.append([book['title'],
        book['publish_date'][0], book['publisher'][0],
        book['author_name'][0], ''])
```