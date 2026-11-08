# Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: “In this case, we will be creating a private pool of workers that have the e2-standard-2 machine type, with 100 GB of network-attached SSD, and located in us-west1.”

A block of code is set as follows:

```
```
  # Docker Build
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t',
           'us-central1-docker.pkg.dev/${PROJECT_ID}/image-repo/myimage',
           '.']
```
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
```
...
INFO[0002] No cached layer found for cmd RUN npm install
INFO[0002] Unpacking rootfs as cmd COPY package*.json ./
requires it.
...
INFO[0019] Taking snapshot of files...
```
```

Any command-line input or output is written as follows:

```
```
$ project_id=$(gcloud config get-value project)
$ vpc_name=packt-cloudbuild-sandbox-vpc
```
```

Bold: Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in bold. Here is an example: “Select System info from the Administration panel.”

Tips or important notes
Appear like this.