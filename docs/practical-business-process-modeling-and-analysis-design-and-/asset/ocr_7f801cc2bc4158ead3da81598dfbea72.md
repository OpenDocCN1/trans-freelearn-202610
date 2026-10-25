A multi-instance marker can also have a **sequential** variant, indicated by three horizontal lines. This means that the work cannot be divided among different people who do it in parallel (which is what the standard MI version assumes), but that it must be done sequentially. If you are not sure how to remember which marker version is for parallel and which for sequential, there’s a great trick that I learned from Filip Stachecki from EduMAX: the parallel variant has three vertical lines in its name (three **l**s).

# # Extending BPMN

As you’ve seen, BPMN offers you many possibilities. It is an easy-to-use standard notation that can be used by both business and IT users. There are hundreds of tools that support it, and lots of learning materials that can help you deepen your knowledge.

However, even a 500+ page specification of BPMN could not cover all business modeling scenarios (especially when you take into account that the BPMN 2.0 specification also covers so-called **choreography diagrams** and **conversation diagrams**—two additional types of diagrams that are not very popular). The authors clearly state that the scope of BPMN is about the processes and not about concepts such as organizational modeling, data and information modeling, and strategy modeling, as well as modeling business rules, process simulation, and monitoring. They do not even mention other important scenarios, such as risk and compliance management or process architecture.

Luckily for us, BPMN contains the brilliant concept of extensions. It is possible to add new attributes, markers, or artifact shapes, so long as the changes do not conflict with already existing elements. Extensions are also saved in a special way in XML BPMN Diagram Interchange files.

# # BPMN extensions in practice

BPMN extensions allows tool vendors to store their extensions inside BPMN files (such as form definitions and other elements needed for process automation), but they also provide some add-ons for standards such as BPMN in Color and BPMN i18n (as you’ve probably guessed, the former allows colors to be exchanged in BPMN diagrams, while the latter provides support for internationalization, which involves storing diagram translations that are in several languages in a single file).

I want to share a simple example taken from the reference files provided by BPMN MIWG at `https:// github.com/bpmn-miwg/bpmn-miwg-test-suite/tree/master/Reference`. If you would like to explore more than a small snippet of the BPMN XML taken from a full file, please check out the `C.8.0.bpmn` file:

```
`<semantic:sendTask id="_a97cla48-faba-447b-bfa6-7aa81a6fe0a0"
name="Notify Employee of Approval" triso:readOnly="false"
implementation="##WebService">
<semantic:extensionElements>
<adonis:MailTo>${email}</adonis:MailTo>
<adonis:MailSubject>Your Vacation Request ${SYS.PROCESSINSTANCE_ID}
was approved manually</adonis:MailSubject>`
```