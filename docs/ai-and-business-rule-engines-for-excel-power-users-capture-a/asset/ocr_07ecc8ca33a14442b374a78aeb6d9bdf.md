# # Make your decision service easy to work with

You might not have realized it, but in the previous chapter, you wrote your first decision service. When running our *Greeting Example* in the KIE Sandbox, we supplied inputs (a name and if it was morning or not), made a decision, and then shared the output (**Good Morning** or **Good Afternoon**).

While we can’t see many colleagues signing up to use this particular service, this will change quickly as we move into the more sophisticated examples in this chapter. It’s worth thinking now about what other people need to know when using our service:

+   - How we make a decision – in general, the **Decision Model and Notation (DMN)** format makes this pretty clear. It is certainly much clearer than other solutions (such as functions hidden in code or in Excel).
+   - The data needed to be passed into the decision service, and the format of the data we get back from it.

In our greetings service, we expected a String (Text name) and Boolean (whether it was morning – true or false). But Decision Models allow us to be a lot more specific about the type of data we pass in and out. This is a good thing, as it makes it easier for people to use our service.

Before we begin to specify the data we expect to pass into, and back out from our Decision Service, it’s worth thinking about how Excel models data.

# ## What shape is your data – tables or trees?

Excel is brilliant – you’ve never really needed to think about the shape or structure of your data. All Excel data is table based on a grid-type layout. A typical example would be *Figure 4.1* – a table of customer information that we might have if we were running an online chocolate shop:

| Customer Number | Customer Name | Date of Birth | Country of Residence | Special Request |
|---|---|---|---|---|
| 1029 | Jane Smith | 10-Mar-2001 | Netherlands |  |
| 2219 | Sita Patel | 18-Apr-1998 | New Zealand | Vegan |
| 8781 | Joseph Okoru | 22-Sep-1963 | Sri Lanka |  |
| 1123 | Peter Murphy | 04-Nov-02 | United States |  |

*Figure 4.1 – A very simple customer table*

It’s pretty simple, and in real life, we’d have more columns than the ones listed here – perhaps to store emails, shipping addresses, and similar information. But it’s good enough to demonstrate the grid-type layout that Excel encourages you to use. And there are obviously more sophisticated sheets – for modeling company financial projections, where an entire sheet would represent one company, instead of the one-row-per-client layout we show here. But our simple example is enough to show the limitations of grid-based data.