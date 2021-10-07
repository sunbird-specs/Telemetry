# html\_interface\_library

## Need

The HTML interface library eases the HTML developer’s effort to log telemetry events from HTML content. It uses simple API methods exposed by the library to log associated events. The reasons to develop the HTML interface library are:

* Any HTML content can package the interface library as part of the content. Thus using the library, the HTML content can log telemetry events.
* There are simple API methods to generate the complete telemetry event as only the required fields are passed
* It is easy to upgrade to new versions, in case of major changes in the telemetry library
* There is effortless backward compatibility, as changes are handled within the telemetry library. Any upgrade of the telemetry library does not require code changes in the content
* The library will handle logging of events from HTML content when it is playing in case portal or device/app.

## How to use HTML interface library

Add the following to your HTML Content:

The file\_path is the relative path \(eg. assets/js\) to these files within the html content.

```text

<!-- HTML Interface  JS library -->
<script src="[relative_path]/htmlinterface.js"></script>

//you can log telemetry interact event as shown below
org.ekstep.contentrenderer.interface.telemetryService.interact(data) 
//or 
RI.telemetryService.interact(data)
```

## API methods

The HTML interface provides simple methods to log telemetry, to handle the ContentRenderer overlay, to get content information, etc.

The HTML interface exposes the following list of API methods:

* [dispatchEvent](html_interface_library.md#dispatchevent) - This method helps dispatch events to the ContentRenderer to handle specific functionality of ContentRenderer
* [getcontentMetadata](html_interface_library.md#getcontentmetadata) - This method is used to access content metadata
* [getConfig](html_interface_library.md#getconfig) - This method is used to access content-renderer configuration
* [gotoEndPage](html_interface_library.md#gotoendpage) - This method helps to open the ContentRenderer end page after HTML content is completely viewed
* [exit](html_interface_library.md#exit) - This method helps to close the ContentRenderer
* [telemetryService.interact](html_interface_library.md#telemetry-interact)- This method helps to log telemetry interact event
* [telemetryService.impression](html_interface_library.md#telemetry-impression) - This method helps to log the Impression event on page or state change
* [telemetryService.response](html_interface_library.md#telemetry-response) - This method helps to log telemetry response event when an option is selected during an assessment
* [telemetryService.assessmentStart](html_interface_library.md#telemetry-assessmentstart) - This method is used when an assessment begins and it returns the event object
* [telemetryService.assess](html_interface_library.md#telemetry-assess) - This method helps to log the Assess event when the assessment is evaluated
* [telemetryService.exdata](html_interface_library.md#telemetry-exdata) - This method helps log the telemetry ExData event

### DispatchEvent

Dispatch an specific event to control ContentRenderer functionalities.

```text

/**
  * eventName - Event name that has to dispatch to handle ContentRenderer functionality
  */
dispatchEvent: function(eventName) {
  // dispatch event to control ContentRenderer functionalities
}
```

### getcontentMetadata

This will return the content metadata information\(HTML Cotnent metadata here\).

```text

/**
  * contentId - Current opened HTML content identifier
  * cb - callback function after after getting content iformation from API call
  */
getcontentMetadata = function(contentId, cb){
}
```

### getConfig

This will return the content renderer configuration. This will help to know what is the context of HTML content playing in ContentRenderer.

```text

getConfig = function(){
}
```

### gotoEndPage

After completion of HTML content, you can call this function to show ContentRenderer end-page. This will take the user out of HTML content view.

```text

gotoEndPage = function(){
}
```

### exit

If you want to close the HTML game & contentRenderer to take user back to app, you can call this function.

```text

exit= function(){
}
```

### Telemetry Interact

Api method to log telemetry interact events. Any interact events in HTML can log using this API method.

```text

/**
 * Interface to log temetry interact(INTERACT) event
 * data - {Object} Telemetry event data
 */
interact: function(data){
  _telemetryService.interact(data.type, data.id, data.extype, data.eks);
}
```

### Telemetry Response

Api method to log telemetry response events. When an assessment is playing in content, on selection of option/answer data can be passed by calling this function.

```text

/**
 * Interface to log telemetry response(RESPONSE) event
 * data - {Object} Telemetry event data
 */
response: function(data){
    _telemetryService.interact(data.type, data.id, data.extype, data.eks);
}
```

### Telemetry assessmentStart

Api method to get assessment start event data. This event object should be passed as a parameter while calling telemetry assess api method.

```text

/**
 * Interface to get assess start event
 * data - {Object} Telemetry event data
 */
assessmentStart: function(data){
    _telemetryService.assess(data.qid, data.subj, data.qlevel, data.data);
}
```

### Telemetry Assess

Api method to log telemetry assess event. After submitting/validating the result of assessment question, call this funciton to log assessment result data.

```text

/**
 * Interface to log telemetry assess(ASSESS) event
 * event - {Object} telemetry event object returned after calling assessmentStart() API method
 * data - {Object} Telemetry event data
 */
assess: function(event, data){
    _telemetryService.assessEnd(event, data);
}
```

### Telemetry Impression

Api method to log telemtry impression event. When there is a state/page change, call this method to log impression event

```text

/**
 * Interface to log telemetry impression(IMPRESSION)  event
 * data - {Object} Telemetry event data
 */
impression: function(data){
    _telemetryService.impression(data.stageid, data.stageto, data.data);
}
```

### Telemetry Exdata

Api method to log telemtry exdata events. Any additional information of can be passed by calling this function.

```text

/**
 * Interface to log telemetry Exdata(EXDATA) event
 * data - {Object} Telemetry event data
 */
exdata: function(data){
   _telemetryService.xapi(data);
},
```

