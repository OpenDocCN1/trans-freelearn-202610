
## Cleaning up the code

Since the list of events and the event form source code could eventually be reused in multiple places on the website, we can refactor the code by creating separate components for each.

In the `web/src/components` folder, create two files: `event-list.js` and `event-form.js`. Both files are similar in format.

The basic format of the event list module is as shown in the following code block:

```
import { Link } from "gatsby";
import React from "react";
const { format } = require('date-fns')

function EventList(props) {

    return (
      ...
    )
}

EventList.defaultProps = {
    title: '',
    eventNodes: [],
}

export default EventList
```

The basic format of the event form module is as shown:

```
import React from "react";

function EventForm(props) {
    return (
      ...
    )
}
```

```
EventForm.defaultProps = {
  title: '',
  venueNodes: [],
}

export default EventForm
```

The source code of the event form and event list will be inserted into return. Opening and closing <div> tags are added since JSX requires a root node:

```
return (
  <div>
    <h2 className='font-serif text-4xl font-bold text-gray-800 bb-10'>{props.title}</h2>
    <div className='max-w-full'>
      ...
    </div>
  )
```

To use the two newly created components, they need to be imported:

```
import EventList from '../components/event-list'
import EventForm from '../components/event-form'
```

Now, the event list and event form may be replaced with these components. The nodes and title will be passed into these components as properties:

```
<EventList
  eventNodes={eventNodes}
  title='Events'
/>

<EventForm
  venueNodes={venueNodes}
  title='Add an Event'
/>
```

```
</label>
</div>

<div className='mt-10 block'>
  {isVisible && <label className=''>Event Venue:
    <select className='form-select block w-full mt-1' name='venue' id={`venue`}>
      {props.venueNodes &&
        props.venueNodes.map((venue) => (
          <option value={venue._id}>{venue.name}</option>
        ))}
    </select>
  </label>
}
</div>
```

The HTML onClick event attribute is added to the toggle input, passing the opposite value (!) of the isVisible Boolean variable:

```
onClick={() => toggleVisible(!isVisible)}
```

If isVisible evaluates as true, then the event venue selection and label are shown; otherwise, they're hidden.

## Summary

In this chapter, we learned how to style a website for modern design and responsiveness using the Tailwind CSS framework, saving time and effort when compared to starting with traditional CSS. Next, we learned how to extract fragments of the page into separate components. Finally, we added dynamic functionality to the form itself.

Styling and polishing the user experience is a deep subject, and therefore beyond the scope of this book. Feel free to read the Tailwind CSS documentation for more ways to improve the appearance. Also, even using other elements of React, which Gatsby can fully take advantage of, could help build out this prototype into a full website—you have all the tools needed.

In the next chapter, Netlify will be used to deploy the website and we will see its usefulness in automating many tasks that previously needed to be performed manually.

## 14 Deployment Using Netlify and Azure

In the last chapter, we prepared our website for deployment. We added style to the components using Tailwind CSS. Lastly, we cleaned up the layout and refactored the code to components. Now, we can look more closely at the last component of the Jamstack used in this book: **Netlify**. Netlify is used as the default platform for the *Blog with Gatsby* example that was created at [https://sanity.io/create](https://sanity.io/create). At the time of writing this book, Microsoft has added the Static Web Apps service, providing yet another option for deployment. Since GitHub and Azure are both owned by Microsoft, there is an excellent integration between the Static Web Apps service and GitHub.

This chapter first introduces the Netlify serverless continuous deployment hosting service, which automates many of the tasks that previously either needed to be developed or performed manually, and then demonstrates Azure Static Web Apps as an alternative for developers who may already be using Azure or who want to try this new service.

The main topics that we will cover in this chapter are the following:

- Introduction to Netlify
- Netlify plugins
- Advanced configuration through the netlify.toml file
- Netlify deployment via the command line
- Azure Static Web Apps deployment

### Technical requirements

To deploy the project on Netlify, you must have an account at https://www.netlify.com/. To deploy the project on Azure, you must create an account at https://azure.microsoft.com/en-us/.

Additionally, a Terminal program is needed.

### Introduction to Netlify

Netlify, to someone who is new to web development, initially looks like a deployment and hosting platform.

In fact, a Netlify website can be a simple static file web host. Using Netlify's drag and drop feature, a folder of web files can be deployed as a real, functional, and hosted website in seconds, as shown:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_214_0.png)

Figure 14.1 – Netlify's drag and drop deployment interface

However, while it is a place to host your website, it is so much more than that.

Firstly, behind the scenes, when we initially created our Sanity and Gatsby-generated website, the code provided by Sanity and Gatsby Blog Template automatically runs it, thereby creating an integration between Sanity and Netlify.

Both Sanity Studio and the Gatsby-generated website were immediately built and hosted on Netlify’s server, and new versions of the website could be easily and automatically deployed as often as we wish. Through integration connecting Sanity’s management tool and Netlify, a new version of the website can be built when we change the contents within Sanity. Also, there is a hook or a trigger set up through GitHub that will automatically deploy a new version of the website whenever the Gatsby source code is pushed to GitHub.

Secondly, Netlify is a serverless continuous delivery service built on top of Amazon Web Services (AWS). This means that all the infrastructure provided by AWS is automatically built into Netlify’s hosting ability. This means that there are servers located throughout the world, also referred to as the cloud, that deliver content in the fastest way possible to all website users.

However, these two aspects are just the beginning of the story. Netlify can run many different types of code together through scripts, in several languages, during the build process to enhance your website, as shown in the following screenshot:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_215_0.png)

Figure 14.2 – An excerpt of Netlify’s build output

This is useful when there are pre-built pieces of code that are needed to build the static pages. For example, the PHP programming language can be used to extract or preprocess format data or even generate websites statically. Hence, the JavaScript programming language is not the only option during deployment.

It is important to remember that code can only be run on Netlify at the time of deployment. Hence, a traditional content management system such as WordPress cannot run on Netlify since it requires server-side code to run on each page request.

One nice feature is the ability to do an A/B test. This is accomplished by using two branches in GitHub. Let’s imagine that you would like to test the effect a particular color has on the performance of a button that website visitors need to click. You could have two different branches in GitHub configuring Netlify to randomly select one or the other.

Another feature that Netlify provides is an easy way to insert code snippets into the header or body of a page. One example of this usage would be pasting the code snippet required for Google Analytics to work properly in the page.

Next, let’s learn about ways to take advantage of what Netlify has to offer though their plugins.
