
- **AI will generate a lot of content**: AI can generate a high volume of content, so we will see a flood of AI-generated text, pictures, videos, websites, and more. This content will likely vary widely in quality, utility, and accuracy. This will resemble the explosive growth of email resulting from the low cost of sending emails, including everything from high-value messages to junk (spam) email and everything in between (newsletters, marketing, group email, and more).
- **Security implication**: This explosion of convincing and personalized content of questionable value will require mitigations such as user education and specialized products to measure the quality, accuracy, and utility of content.
- **AI will generate a lot of confusion**: As AI usage grows, people will get confused by content created by AI such as convincing “deepfake” impersonations of people in video, voice, and email conversations. Additionally, many people will misunderstand what AI technology can and can’t do because they don’t have a good reference point – the current AI technology is very different than how AI has been portrayed in movies and TV shows for decades. Some people will be overly excited and exuberant about using the technology while others will be overly fearful and cautious with this technology – each extreme can create unnecessary risk.
- **Security implication**: Organizations should urgently focus on education, managing expectations, security/financial processes and controls, and setting clear policies.
- **AI increases the need for data-centric security**: AI models need good data, dramatically increasing the focus on data security and data quality. Every organization’s internal data quickly increased in value because AI models need human-generated data and the internet is already flooded with AI-generated content.

AI models trained on AI-generated data result in very low-quality results (similar to how a photocopy of a photocopy is of lower quality), a dynamic called **model collapse**. *Figure 5.1 illustrates model collapse by showing the difference between training an AI model on human-generated data versus AI-generated data:*

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_85_0.png)

Figure 5.1 – Illustration of model collapse for models trained on AI-generated data

Because of the value of AI models and the need for original human-generated content to train them, human-generated data such as the internal data in most organizations becomes extremely valuable for AI models. This further increases the value of data, and we are already seeing attackers target organizations to steal their data for this reason.

- **Security implication**: Organizations must urgently adopt a data-centric Zero Trust security approach to protect their data and their newly discovered competitive differentiator. Traditional network-centric security approaches will not be able to effectively protect data against attacks.
- **AI success will inspire further disruption**: The successes of generative AI will also encourage investment and funding of additional technological innovations and disruptions. As organizations embrace AI technology, they will apply it to more and more scenarios that push the boundaries of what computers can do, such as measuring and understanding people's emotional states (sometimes called affective computing). Each of these advances will generate more conversations and requirements for guidance, policy, education, and more.
- **Security implication**: Business, technology, and security teams will need to work together to understand these changes and build policies and controls that allow the organization to responsibly adopt these technologies. Organizations will have to constantly learn how to manage these disruptions so that they enable the business to grow and thrive while reducing risk. The agile security approach of Zero Trust will be a key asset to do this.

As you can see, AI will have widespread impacts that require business, security, and technology teams to work together using a Zero Trust approach. The per-role guidance in each playbook goes into more detail on the implications of AI for each role.

## AI transformation will resemble digital transformation

The AI transformation at your organization will be similar to your digital transformation in many ways. Each transformation will replace existing business processes with new processes based on more advanced technology that is more effective and efficient. Both transformations will also be a source of continuous change and disruption from both external organizations and internal innovation.

For additional information on how to think about various types of AI risk, the NIST AI Risk Management Framework is a good resource: https://www.nist.gov/itl/ai-risk-management-framework.

While AI will create many new opportunities and risks, there are very real limitations regarding what AI technology can currently do.

## What are the limitations of AI?

AI technology will be powerful and transformative, but AI is not a magic silver bullet solution for security or any other problems. AI has very real and significant limitations, and it is far from the fully independent human-like **artificial general intelligence (AGI)** we frequently see portrayed in movies and TV shows.

The main limitation is that AI models don’t truly understand content in the complex and rich ways that humans do. AI will also naturally reflect and propagate any biases in its data, whether or not these biases are known and documented.

Let’s take a quick look at these key limitations of AI.

## AI models do not “understand” anything

While generative AI appears to “understand” things on the surface, it is only imitating the original content it was trained on. This extremely shallow “understanding” is similar to fake buildings (façades) on a Western movie set – these façades look like buildings to carefully placed cameras but don’t function as buildings at all.

*Figure 5.2 illustrates the difference in how a human versus an AI sees and understands something:*

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_87_0.png)

### Humans have a deep and rich understanding of a house

- Emotional association with home
- Bought/sold as real estate
- May be rented for income
- Made of wood, concrete, other materials
- Contains bedrooms and bathrooms
- Requires regular maintenance

AI has an extremely shallow “understanding” based on other pictures of houses

Figure 5.2 – Artificial intelligence versus human intelligence

While humans can naturally infer rich meaning and facts from content such as a picture of a house, AI can only recognize and generate new content that's similar to what it was trained on. The contents (documents, pictures, videos, and so on) may appear realistic, but they are just shallow representations of the underlying subject:

- Visual models efficiently analyze and generate visual content, but they don't fully understand the objects in the picture and how they interact with the **three-dimensional** (**3D**) world around them.
- Language models know how to analyze and generate text, but they don't natively understand the ideas, concepts, or models in the text. They can only predict what words are likely to come next in a particular context (a document, article, poem, movie script, or other format).

During this process, AI models also often make AI-specific errors that humans would never make. For example, the first releases of visual models often get things such as fingers and hands wrong when generating pictures (too many or too few fingers, unnatural proportions, unnatural positions, and more). This is because the visual models don't understand fingers and hands like a human would (hands typically come with four fingers and a thumb, finger length comes in certain proportions, fingers can only move in certain ways, and so on). The visual models only “understand” the visual depiction of patterns in its training data.

Identifying and correcting these errors (or preventing them in the first place) requires a human doing **quality assurance (QA)** or using an automated QA mechanism.

## AI capabilities and limitations will change over time

Over time, many of the limitations of AI will evolve as use cases are identified and found to be helpful, profitable, or otherwise beneficial.

Researchers and developers will focus on identifying and resolving AI limitations related to those use cases such as correcting the depiction of fingers and hands, providing technical or medical QA terminology in document generation using existing databases and document repositories, correcting software security issues in code, and more.

**Security implication**: Organizations will need policies and controls to ensure quality, accuracy, security, privacy, ethical use, and other required attributes. The policies and controls should cover the organization's own AI applications as well as the organization's use of external AI applications.

## AI models reflect any biases in their data

Additionally, AI often replicates biases in the data that it processes, including the original training data and other data sources it's connected to, as well as any biases in the chat conversations
