
## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Business-Rule-Engines-and-AI-for-Excel-Power-Users. If there’s an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://packt.link/EAEVJ.

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: “The formula to match with the first 10,000 customers is Customer.Number < 10000.”

A block of code is set as follows:

```
let
    // "SourceUrl" with quotes needs to match the named range on
    our Excel sheet. You may need to change {1} to {0} depending on
    when your first line begins
    pSourceUrl = Excel.CurrentWorkbook() { [Name="SourceUrl"] }
    [Content]{1}[Column1]
    in
        pSourceUrl
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
let
    // "SourceUrl" with quotes needs to match the named range on
    our Excel sheet. You may need to change {1} to {0} depending on
    when your first line begins
    pSourceUrl = Excel.CurrentWorkbook() { [Name="SourceUrl"] }
    [Content]{1}[Column1]
    in
        pSourceUrl
```

Any command-line input or output is written as follows:

```
curl -X 'POST' \
  'https://dmn-dev-sandbox-yu88rl6qu490-crt-openshift-dev.apps.sandbox.x8i5.p1.openshiftapps.com/Customer Recommendations/Product Recommendation Service' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
"Customer": {
  "Number": 0,
  "Name": "string",
  "Date of Birth": "2022-10-23",
  "Country of Residence": "string",
  "Special Requests": "diabetic",
  "Previous Orders": [
    0
  ]
}
}'
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in **bold**. Here is an example: “Click **Done** at the bottom right of the screen to save the code and exit the screen.”

Tips or important notes
Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, email us at `customercare@packtpub.com` and mention the book title in the subject of your message.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit `www.packtpub.com/support/errata` and fill in the form.

## Download a free PDF copy of this book

Thanks for purchasing this book!

Do you like to read on the go but are unable to carry your print books everywhere?
Is your eBook purchase not compatible with the device of your choice?

Don’t worry, now with every Packt book you get a DRM-free PDF version of that book at no cost.

Read anywhere, any place, on any device. Search, copy, and paste code from your favorite technical books directly into your application.

The perks don’t stop there, you can get exclusive access to discounts, newsletters, and great free content in your inbox daily

Follow these simple steps to get the benefits:

- 1. Scan the QR code or visit the link below

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_24_0.png)

```
`https://packt.link/free-ebook/9781804619544`
```

- 2. Submit your proof of purchase
- 3. That’s it! We’ll send your free PDF and other benefits to your email directly

## Part 1:
The Problem with Excel,
and Why Rule-Based AI
Can Be the Solution

The first part of the book builds the case, showing how an AI and business rules approach can help solve the problems you are encountering in Excel.

This section includes the following chapters:

- *Chapter 1, Wrestling with Excel? You Are Not Alone*, outlines the problems you may be facing in Excel, but reassures you that you are not the only person to encounter these issues and shows how an AI-based rule approach can help.
- *Chapter 2, Choosing an AI and Business Rules Engine – Why KIE and Drools?*, shows why the rules engine and AI tools from the KIE project are the best choice for you as an Excel user and welcomes you to an open community supported by IBM and Red Hat where you can get ongoing support.
- *Chapter 3, Your First Business Rule with the Online KIE Sandbox*, dives into writing business rules and creating your first decision model in the online KIE Sandbox using a classic Hello World example.

## Wrestling with Excel?
You Are Not Alone

If you read the Preface, you will know that a lot of people use Excel, being one of them, and that you are not the only person frustrated with the limitations of it as a tool.

Based on the fact that you have read on, you must now know that this book can help you. But before we can go into how exactly it can help, it’s worth exploring in detail the problems we’re trying to solve. In this chapter, we’ll cover the following:

- Why do you use Excel?
- Enterprise solutions and the gap between them and Excel
- What’s your real business problem?
- What is artificial intelligence and which type can help you best?
- Practical artificial intelligence and business rules
- Splitting Excel into different pieces

## Why do you use Excel?

If you ask most of the billion Excel users around the globe, most of them will say they use Excel for these two main reasons:

- As a convenient place to store information – more structured than using post-it notes, but flexible enough to evolve over time
- A convenient way to exchange information – because everybody else uses it

If you are an Excel power user, you’ll add a third reason:

- You use Excel to process important information. And your organization depends on the knowledge you have in your head to get the process right

In working with Excel, you and your colleagues are using it as the core engine to drive your business. Let’s look at a couple of real-world examples.

## Excel as the engine room of the business

There are many important jobs in every business (I’m using *business* as shorthand for *any organization*). But those jobs will grind to a halt without a way of capturing, processing, and sharing information. On the surface, it looks like Excel does these tasks very well:

- If you work for a medical company, you might use Excel to capture orders for vaccines, process them to forecast demand, and then use Excel to share orders with various plants worldwide
- If you’re in the aircraft leasing industry, you might use Excel to track how many planes you have placed with airlines worldwide, process the information to calculate different risk scenarios (for example, a major war breaking out), and then use Excel to share information with the banks that are funding you
- If you’re a small business, you could use Excel to track salary information, process it to calculate payroll taxes due, and carefully share the relevant information with each employee

The following diagram shows how you use Excel in each case:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_28_0.png)

Figure 1.1 – How you use Excel

This diagram is so simple that you’ve probably never needed to think about it – you take some information in (for example, from a spreadsheet), process it (apply hand-crafted formulas or review it manually), and then share it (maybe save it to an internal drive or SharePoint for others to use).

And that is the core of the problem – your processing is bundled up with how you gather and share your information. This workflow is great for getting started, but sooner or later, you’re going to run into problems as each of the steps in Figure 1.1 gets in the way of each other. We’ll look at how you might encounter this problem in your daily work, before beginning to talk about ways of solving it.

## Day-to-day Excel problems

How you use Excel is unique to you. But I’m guessing that you’ve run into several of the following common problems:

- A large part of your day involves repeating tasks in Excel by hand. This includes capturing information by manually copying details from one sheet to another.
- Emailing Excel files back and forth, not being sure whether you’re using the latest version, and losing information by editing the wrong version.
- Wondering whether your formulas may have been broken by other users and spending hours rechecking them.
