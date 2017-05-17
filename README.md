# Language Resource Switchboard Hackathon

This is a repository containing information, links and files to be used in relation to
the Language Resource Switchboard (LRS) Hackathon to take place on 17 May 2017 at the
[CLARIN Centre Meeting 2017](https://www.clarin.eu/event/2017/centre-meeting) in Utrecht, 
The Netherlands.

Organisation: [Claus Zinn](https://github.com/claus-zinn) (University of Tuebingen) 
and [Twan Goosen](https://github.com/twagoo) (CLARIN ERIC).

**Content**
* [Schedule](#schedule)
* [Background](#background)
* [Instructions](#instructions)
* [Links](#links)

## Schedule

Wednesday 17 May 2017
- __9:00__ Introduction/instructions
- __9:20__ Round of participants & tools
	- ~1 minute per participant: describe tool, input/output, programming language
- __9:30__ Coding
- __10:30__ Coffee break
- __11:00__ Coding
- __12:00__ Presentations & discussion
- __12:15__ Discuss next steps
- __12:30__ End of hackathon

## Background

The Language Resource Switchboard (LRS) gives users with a language resource easy access 
to tools that can process the resource. The LRS is more than a Yellow Pages directory, 
however. Given a resource's language (e.g., English, Dutch) and its mime type 
(e.g., text/plain) it lists all tools capable of processing the resource, and sorts 
them in terms of the tasks they promise to achieve (e.g., constituency parsing, named 
entity recognition). Once the user selects a tool from the list, the LRS starts the tool. 
Here, it is the aim that the user, once directed to the tool, does not need to re-enter 
any information known to the switchboard (language, mime type, task). Rather, the tool 
shall start with the given context (but users are free to enter - tool specific - 
additional configuration parameters). For this to happen the LRS invokes the tool in 
question via an HTTP request where all relevant parameters are URL-encoded. More details
can be found in [CLARIN-PLUS deliverable D2.5](https://office.clarin.eu/v/CE-2016-0881-CLARINPLUS-D2_5.pdf).

To integrate a tool with the LRS switchboard, the following tasks need to be tackled:

* Give the LRS metadata about the tool
* Modify the tool
	* Being able to parse the URL used for tool invocation (reading parameters and their values)
	* Advances the state of the tool by taking the parameters' values into account
		* e.g, if the tool's UI had a pull-down menu for language selection, then it 
		should now show the language information passed in the URL

How to get started doing is is explained in the following section.

## Instructions


### 1. Find sample resources

The easiest way to get started is by finding some resources that match the input supported
by your tool in terms of language and media type. See the 
[overview of sample resources](samples) or browse the selection of resources in the  
[Alpha VLO](http://alpha-vlo.clarin.eu/hackathon)
to see what is available (use the 'Language' and 'Format' facets to quickly find resources
matching your tool). You can also go to the [connected B2DROP instance](http://weblicht.sfs.uni-tuebingen.de/owncloud) (ask for credentials) and see what is available there, or upload your own resources for testing.

### 2. Invoke the LRS

Invoke the LRS, which can be done in three ways:
- From the [Alpha VLO](http://alpha-vlo.clarin.eu/hackathon). You can invoke the LRS on an individual resource by going to a record page for a search result, selecting the _Resources_ tab and then choosing the LRS option from the action menu (described in detail on the [VLO's help page](https://vlo.clarin.eu/help#processing-resources)). 
- Via [B2DROP](http://weblicht.sfs.uni-tuebingen.de/owncloud). Ask for the credentials and log in. Make sure that a resource is shared before calling the LRS on it via the context menu.
- Or the [stand-alone version](http://weblicht.sfs.uni-tuebingen.de/clrs-dev/). In this case, you will have to upload a file yourself, for example one of the [sample resources](samples).

Have a look and see what processing options the LRS has to offer. Also notice that as a
user of the LRS you can tweak the input settings (language and media type) or provide
missing information about the resource if necessary.

### 3. Invoke your tool with a (sample) resource

The LRS invokes tools by means of a redirect to a tool-specific, parameterised URL. It has
metadata per tool that defines the URL and the parameter name(s). Try invoking your tool
with one of the resources you discovered in the previous step, passing in language and 
mime type information if applicable and supported by your tool. You may find that your
tool needs to be adapted in order to support this, which would be the next step!

### 4. Adapt your tool (if necessary)

Make sure that your tool can do the following, or make the necessary adaptations:
* Provide an endpoint at which it accepts a resource for processing via a URI passed 
through a query parameter in URL encoded form. An example of such an endpoint URL invoked
with a resource URI:
`http://tools.my-organisation.edu/mytool/process?resourceUri=http%3A%2F%2Fwww.resource-provider.org%2Fresource.txt`.
	* Also think of supporting PIDs! Resource URIs could be of the form 
	`http://hdl.handle.net/1839/00-0000-0000-0005-62D4-0` or
	`hdl:1839/00-0000-0000-0005-62D4-0`
* Immediately offer an interface aimed at human users that immediately offers processing
results or makes clear how to proceed. If there is a need to provide additional parameters,
this should be possible at this point.
* Provide processing results as one or more download options, or show them directly in the
browser window.

Preferably tools can be used without the need to authenticate. If authentication is
required nevertheless, make sure that the parameters 'survive' the authentication step
and there is a seamless experience for the user after completing it, i.e. there should be 
no need to re-enter information about the resource that was already known at the point of
reaching the LRS.

Remember that the primary goal of this hackathon is to make something that works. If
you reach a minimal level of support, go on to the next step and connect your tool to
the LRS. You can come back to this step and make improvements later!

### 5. Connect to the LRS

Tool **metadata** can be sent  to Claus via email. The following information is required:
* Language(s) of the resource it can process
* Mime type(s) of the resource it can process
* The task it promises to achieve
* The URL where the tool lives
* The parameter names the switchboard should use to invoke the tool

It is stored as a nested JSON structure. More information during the hackathon, or via the 
[LRS homepage](http://weblicht.sfs.uni-tuebingen.de/clrs) under _Developer_, or have a look
at this [example](samples/lrs-metadata/lrs-metadata.json).

### 6. Integration testing

__Finally__: once the metadata has been added to the (development) LRS, try invoking your tool with
an applicable (sample) resource!

## Links
* [CLARIN Language Resource Switchboard](http://weblicht.sfs.uni-tuebingen.de/clrs) - Primary instance of the LRS, running in Tübingen
* [CLARIN Language Resource Switchboard - development version](http://weblicht.sfs.uni-tuebingen.de/clrs-dev) - Latest development version of the LRS, best to use this during the hackathon
* [Alpha VLO with sample resources selected](http://alpha-vlo.clarin.eu/hackathon)
* [B2DROP instance connected to the development LRS](http://weblicht.sfs.uni-tuebingen.de/owncloud) (ask for credentials)
* [LRSwitchboard repository](https://github.com/clarin-eric/LRSwitchboard) - LRS source code
