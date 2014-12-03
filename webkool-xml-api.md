# Webkool 1.0 - XML api reference
*Draft version 0.1*, *December 1st, 2014*

## **1. About this document**

This document provides details on the elements that make up the XML api of webkool. 

## **2. XML api reference**

This section provides details on the elements that make up the webkool XML API. For each element, a brief description is provided. (For elements described as defining constructors or templates that create or instantiate objects, more information about the objects can be found in the webkool ECMAScript api reference document.) Then the following information is presented if relevant: 

**Tags**

This is the xml element tag. The namespace is always the default webkool namespace.

**Attributes**

This is the xml element attribute. Attributes have one of the type specified in the following table. Attributes that are required are indicated as such in this section. Unless noted otherwise, the default values for optional attributes are as shown in the table.

|Type|Description|Default value|
|----|-----------|-----|
|filename|a string representing a valid filename|*none*|
|identifier|an ECMAScript identifier|*none*|
|mime-type|a string|text/html|
|namespace|a string|http://www.webkool.net/1.0/|
|url|a unique resource locator|*none*|

**Elements**

The XML elements within this element.

**CDATA**

The characters within this element. Several elements hold ECMAScript code, stylesheet or HTML, which is usually escaped like this: 

<![CDATA[

// ECMAScript code, stylesheet or HTML 

]]> 

The individual reference sections are ordered alphabetically for ease of lookup. 

### **2.1 Application Element**

This is the root element of any webkool document.

**Tags**

application

**Attributes**

`xmlns`: *namespace* - *required*, always ‘http://www.webkool.net/1.0/'

**Elements**

client, handler, include, property, server, script, stylesheet, template.



### **2.2 Client Element**

This element is used to specify to the wkc compiler that we are at the client side. Then the element’s child are included only if the file is compiled with the client flag set to true.

**Tags**

client

**Elements**

handler, include, on[1], property, script, stylesheet, template.

[1] The ‘on’ element is accepted only if the client element is used inside a handler element.



### **2.3 Handler Element**

This element is the main tag in the webkool xml api. It declare a handler that will respond to a request depending on its behavior.

**Tags**

handler

**Attributes**

`url`: *url*, *required*, should be unique to avoid conflict. 

`type`: *mime-type*, *optional*, used by the http server to specified the response. the default value is text/html.

`Constructor`: *identifier*, *optional*, identifier of an existing constructor, the default value is Handler.

**Elements**

on, template.


### **2.4 Include Element**

This element is used to include an external webkool file.

**Tags**

include

**Attributes**

`href`: *filename*, *optional*, an existing webkool filename. The path should be relative to current document path, or to the ‘include’ path that are specified in the wkc tool parameters..


### **2.5 On  Element**

The on Element add the event id to its handler behavior. The main event id are ‘request’, ‘complete’ and ‘error’:
* ‘request’ is called when a handler is requested.
* ‘complete’ when all the sub request are done.
* ‘error’ when an error occurred during the sub request.

> There is an special event named ‘render’ that is called when the handler is a root handler (first call handler) to layout the result.

**Tags**

on

**Attributes**

`id`: *identifier*, *required*, should be unique for a specific handler.

**CDATA**

ECMAscript code, the body of the event. The local environment implicitly accessible inside an event are *handler*, *model* and *query* for all event except ‘render’ event. 
The local environment of the ‘render’ event are handler and scope (which is a short cut to handler.result).  



### **2.6 Server  Element**

The element’s child are included only if the file is compiled with the server flag set to true.

**Tags**

server

**Elements**

handler, include, on[1], property, script, stylesheet, template.

[1] The ‘on’ element is accepted only if the client element is used inside a handler element.



### **2.7 Property Element**

This element is used to store application specific parameters values.

**Tags**

property

**Attributes**

`id`: *identifier*, *required*, the property name to store the value.  

**CDATA**

the value to store, it’s store inside application.properties[id].



### **2.8 Script  Element**

This element is used to include inline or external ECMAScript file. The script content are include in the declaration order.  

**Tags**

script

**Attributes**

`href`: *filename*, *optional*, an existing ECMAScript filename. The path should be relative to current document path, or to the ‘include’ path that are specified in the wkc tool parameters..

**CDATA**

The ECMAScript’s body, one of script body or href are required, but not both at the same time.



### **2.9 Stylesheet  Element**

This element is used to include inline or external stylesheet. The stylesheet are include in the declaration order.  

**Tags**

stylesheet

**Attributes**

`href`: *filename*, *optional*, an existing stylesheet filename. The path should be relative to current document path, or to the ‘include’ path that are specified in the wkc tool parameters..

`system`: * identifier*, *optional*, processor name to execute on the file, currently only **lessc** and **sass** are implemented.  

**CDATA**

The stylesheet’s body, one of styleheet’s body or href are required, but not both at the same time.



### **2.10 Template Element**

This element is used to define rendering template. The element could be inside a handler element or at the global level inside an application element. A template inside the application element should have a unique id.

When included in the handler element, template automatically implement the ‘render’ event to layout the content.

Optionally template’s data can be preprocess, the local environment accessible the template language is ‘scope’. 

[SB : Should talk about with(scope)]

**Tags**

template

**Attributes**

`id`: *identifier*, *optional*, the unique template identifier.

`href`: *filename*, *optional*, an existing template filename. The path should be relative to current document path, or to the ‘include’ path that are specified in the wkc tool parameters.

`system`: *identifier*, *optional*, processor name to execute on the file, currently **page** and **mustache** are implemented.

**CDATA**

The template’s body, it could be json, html, plain text,…depending your application.

