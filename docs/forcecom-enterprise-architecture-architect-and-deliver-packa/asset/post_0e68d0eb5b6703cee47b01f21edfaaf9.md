
```
helper.addDrivers(component, event);
}
```

> The c value provider (controller provider) can also be used within JavaScript to obtain a reference to an Apex Controller method the client code wishes to call. We will review an example of this scenario later in the chapter.

## Component Helper (...Helper.js)

A JavaScript file that defines functions are used by the Controller.js and Renderer.js (described as following) files. The framework creates a single instance of these methods which is shared between all component instances. Thus it is recommended you do not store any state and instead leverage the component attributes instead.

This example shows an action binding as referenced by an aura:handler specifying a platform event fired during the initialization of a component. In this case it allows the component to load data from the server dynamically:

```
<aura:handler
 name="init" value="{!this}"
 action="{!c.onInit}"/>
```

The controller function is fired and delegates to the helper class:

```
onInit : function(component, event, helper) {
 helper.getCalendar(component, event);
}
```

The helper function makes a server side call out to the Apex Controller, which is specified through controller attribute within the .cmp file. The c value provider is used here again, but this time to access server side controller methods:

```
getCalendar : function(component, event ) {
 var action = component.get("c.getRaceCalendar");
 action.setCallback(this,
 function(response) {
 if(response.getState() == 'SUCCESS') {
 component.set("v.calendar", response.getReturnValue());
 }
 });
 $A.enqueueAction(action);
}
```

The $A variable used in the preceding code provides access to Lightning JavaScript API.

## Component CSS (.css)

When styling your component, you should leverage the designs and CSS provided by the Lightning Design System, as shown in the mycomponent example earlier. This will ensure your UI continues to stay in line with the Salesforce containers it resides in, as well as, any further advancement in respect to supporting new devices and layout types.

> Using Salesforce base components from the lightning namespace, such as lightning:button, lightning:input and lightning:tabSet automatically ensures you are using SLDS.

However, if you do need to define your own styles, do so via custom CSS classes added to the component's .css file. The framework will ensure that these will be encapsulated within the component and will not affect other components on the page.

> You should not attempt to modify the .style attribute of the element. If you need to control this you may need to implement a Component Render. The framework provides $A.util.addClass and $A.util.removeClass methods to dynamically manipulate the style of your component's markup safely.

The THIS identifier in component CSS must be used to scope the CSS to your component. The following example is used in the **Race Calendar** component to highlight a race when the user clicks on it:

```
.THIS .selectedRace{
    background-color:token(colorBackgroundHighlight);
}
```

The corresponding component markup uses a conditional expression to apply the CSS class (in addition to the SLDS styles). The controller method simply updates the value of the .Selected property from the race binding:

```
<li class="{!'slds-item ' + (race.Selected ? 'selectedRace' : '')}" onclick="{!c.onRaceClicked}">
```

Tokens are a feature of the Lightning framework that allows you to reuse colors and font values expressed by Salesforce in your own styles. The preceding example reuses a token from SLDS that defines what color a selected item is. To include the standard SLDS tokens for use in your components you must create an `aura:tokens` Lightning Bundle, called `defaultTokens`. This has already been included in the sample code for this chapter. In this you can also define your own tokens via the `aura:token` element.

## Component Render (...Renderer.js)

In most cases utilizing the component markup, attributes and bindings to manipulate the appearance of the component is the intended way in which to develop your components. You are also discouraged from manipulating the DOM within your helper or controller methods, as the framework will overwrite the changes when it responds to attribute changes. However, in some cases, for example when using a third party JavaScript library you may wish to take control of the DOM directly. In these cases, utilize a component renderer. Consult the *Lightning Components Developer Guide* for more information on this facility.

## Component Design (.design) and Component SVG (.svg)

These XML based files allow you to customize the appearance of your component when exposing it through the **Lightning App Builder** and **Lightning Community Builder** tools, such as configuring the icon used to identify it in the sidebar, allowing your users to use these drag and drop tools to configure Salesforce UIs with the introduction of your packaged components. See *Making components customizable* later in this chapter for further discussion and examples of this capability.

## Component Documentation (.auradoc)

Lightning provides its own documentation framework that is built dynamically based on the components in the org, including those from Salesforce. If you plan to allow other developers to use your packaged components, you can use this file to place documentation that describes the purpose and features of the component.

You should also leverage the `description` attribute available on `aura:component`, `aura:attribute`, `aura:method`, `aura:event`, `aura:interface` and `aura:registerEvent`.

Developers can access documentation expressed this way via the URL, https://<myDomain>.lightning.force.com/auradocs/reference.app. See Making components customizable later in this chapter for an example and more discussion on how to expose your components for use outside your package.

## Enforcing encapsulation and security at runtime

JavaScript code on a page can traverse the entire page content via the HTML DOM API and obtain information or modify other aspects of the page. This breaks encapsulation at runtime and also breaks security best practices for container architectures such as Lightning. Imagine as an admin you installed a component that you placed on your Home page and it obtained important data and transmitted it back to another server.

While it is unlikely such a component would pass through the Salesforce Security Review and onto AppExchange. There is another reason why in Lightning this type of component is not permitted. Lightning Locker Service, a service designed to physically block this type of logic from executing at runtime, either intentionally or unintentionally has been created by Salesforce. You may unintentionally write code that is considered not secure, by using features that do not comply with the industry standard Content Security Policy (CSP). Features like use of eval and access to the window object are also not supported within the security context the Locker Service creates.

> Locker Service may be activated by default in your org or your customers org. We are thus strongly encouraged to develop and test our code with it enabled if it is not already switched on for your develop and test orgs. There is also a command line utility, Salesforce Lightning CLI, that scans your code for security vulnerabilities. This is well worth investigating and if you have a Continuous Integration process integrating it.

## Expressing behavior

Now that we know how Lightning allows us to encapsulate our implementation. It is important to keep in mind the vision behind components is sharing and reusability, not just within your own application, but by other consumers of your application. As with an Apex class exposing an API, it is important to carefully manage what ways consumers of your components are permitted to interact with them.

## Access Control

Apex uses the visibility keywords to control what classes, properties and methods are visible, within the scope of the class, within a package and outside of it.
