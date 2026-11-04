3. Analyze the mail archives by checking the user mail data or requesting the Exchange server data to identify the mail. We found out that the user received an email from outside the organization with a PDF research article (research_tech.pdf) and replied to the email to thank the sender for the document. You should analyze the timestamp in the mail communication to reconstruct the conversation. This is enough evidence for potential successful exploitation. Note that the email could contain a malicious URL or a callback number requesting actions from the victim, rather than an attached document. In any case, the objective is to identify the intrusion's entry vector. The email sample is shown in the following screenshot:

```
Dear Mr. Stefan,

As member of the EXX group of researchers and impactful individuals,
We would like to let you know of the upcoming research strategy.
The group would like you to speak about the financial impact of Covid-19 for Tech start-ups.

We have attached the research brochure for your reference.
Should you have any question, please do not hesitate to contact me directly using this email.

We would like to express our gratitude for accepting to be part of the upcoming event.

Yours fathfully
Catherine Majabu
Coordinator
EXX group
+27-000010010
```

Figure 10.25 – Simple spear-phishing email example

The email shows that this is a phishing or spear-phishing attack. Contacting the victims and discussing security awareness and phishing tactics is important. The next step is to identify the origin of the email and extract the necessary indicators.

4. Analyze the mail headers to get more indicators, with an illustration in the following screenshot (note the timestamp):