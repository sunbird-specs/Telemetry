# jslibrary

## Need

The purpose of a standalone JS library for telemetry is to facilitate capture and distribution of telemetry data by users who would like to use their own apps, content players or portals.

We chose to use a JS library for the following reasons:

* All the telemetry events that are generated and synced to the server have the same format \(field data types and time zone value\)
* It is easy to upgrade to new versions, in case of major changes in telemetry
* There is effortless backward compatibility, as changes are handled within the telemetry library. Any upgrade of the telemetry library does not require code changes in the content
* There are reduced number of API calls
* There are simple API methods to generate the complete telemetry event as only the required fields are passed

## Prerequisites

The following are prerequisites to use or integrate the JS library:

* JQuery library should be available
* Valid Authtoken and Key to make API calls
* The [telemetry.min.js](https://github.com/project-sunbird/project-sunbird.github.io/blob/dev/pages/developer-docs/telemetry/other_files/telemetry.min.js){:target="\_blank"} file

**Note:** For details on generating and using the Authtoken and Key, refer to the section

* Device ID value

**Note:** For details on how to get the device ID value, refer to [website](https://android-developers.googleblog.com/2011/03/identifying-app-installations.html)

## Configure

This JS library helps to generate telemetry events. These events sync to the server or data-pipeline in a batch as defined in the configuration. To log telemetry events, the user has to call the start method by passing the configuration along with other parameters.

**Note:** All telemetry events sync only to the server or data-pipeline, when connected to the Internet.

Telemetry events are generated based on the configuration of the telemetry library.

**Required Configuration \(Context\)**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Default Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">pdata</td>
      <td style="text-align:left">Producer data. It is an object containing id, version and pid.</td>
      <td
      style="text-align:left">true</td>
        <td style="text-align:left">Defaults to genie ex : {&quot;id&quot;: &quot;genie&quot;, &quot;ver&quot;:
          &quot;6.5.2567&quot; pid:&quot;&quot;}</td>
    </tr>
    <tr>
      <td style="text-align:left">channel</td>
      <td style="text-align:left">It is an string containing unique channel name.</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">Defaults to in.ekstep</td>
    </tr>
    <tr>
      <td style="text-align:left">uid</td>
      <td style="text-align:left">It is an string containing user id.</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">defaults to &quot;anonymous&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">did</td>
      <td style="text-align:left">
        <p>It is an string containing unique device id.</p>
        <ul>
          <li>To generate did value for android refer to <a href="https://android-developers.googleblog.com/2011/03/identifying-app-installations.html">here</a> ANDROID_ID
            is generally used for mobiles</li>
          <li>To generate did value for web client refer <a href="https://github.com/Valve/fingerprintjs2">here</a>.
            If consumer is not sending any did value then by default library will generate
            did using <a href="https://github.com/Valve/fingerprintjs2">fingerPrintJs2</a>.</li>
          <li>For server side it&apos;s mandtory to pass did value</li>
        </ul>
      </td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">Default to <a href="https://github.com/Valve/fingerprintjs2">fingerPrintjs2</a>(Note:
        Only for web client)</td>
    </tr>
    <tr>
      <td style="text-align:left">authtoken</td>
      <td style="text-align:left">It is an string containing consumer token to access the API</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Property</td>
      <td style="text-align:left">Description</td>
      <td style="text-align:left">Required</td>
      <td style="text-align:left">Default Value</td>
    </tr>
    <tr>
      <td style="text-align:left">env</td>
      <td style="text-align:left">It is an string containing Unique environment where the event has occurred</td>
      <td
      style="text-align:left">true</td>
        <td style="text-align:left">defaults to &quot;ContentPlayer&quot;</td>
    </tr>
  </tbody>
</table>

**Additional Configuration**

| Property | Description | Required | Default Value |
| :--- | :--- | :--- | :--- |
| sid | It is an string containing user session id. | optional |  |
| batchsize | It is an int containing number of events count to sync at a time. Can be configured from min value 10 to max value 1000. | optional | defaults to 20 |
| mode | It is an string which defines to identify preview used by the user to play/edit/preview. | optional | defaults to "play" |
| host | It is an string containing API endpoint host. | optional | defaults to "https://api.ekstep.in" |
| endpoint | It is an string containing API endpoint. Please don't change default value. Update this only when the data is proxied | optional | Defaults to "/v3/telemetry" |
| tags | It is an array. It can be used to tag devices so that summaries/metrics can be derived via specific tags. Helpful during analysis | optional | Defaults to \[\] |
| cdata | It is an array. Correlation data. Can be used to correlate multiple events. Generally used to track user flow | optional | Defaults to \[\] |
| dispatcher |  |  |  |

**Sample:**

```text

{
  "pdata": {
    "id": "genie",
    "ver": "6.5.2567",
    "pid": ""
  },

  "env": "ContentPlayer",
  "channel": "XXXX",
  "did": "20d63257084c2dca33f31a8f14d8e94c0d939de4",
  "authtoken": "XXXX",
  "uid": "anonymous",
  "sid": "85e8a2c8-bb8e-4666-a21b-c29ec590d740",
  "batchsize": 20,
  "mode": "play",
  "host": "XXXX",
  "endpoint": "/v3/telemetry",  
  "tags": [],
  "cdata": []
}
```

**Dispatcher:**

User can define custom dispatcher to override the default functionality of telemetry sync. By default telemetry events will send to default server/host. User can override this default functionality by defining his own "dispatcher" object to handle telemetry events.

```text

var customDispatcher = {
    dispatch: function(event){
        // User defined logic to send telemetry to server or store locally etc..
    }
};
```

Send this object as dispatcher in the above sample configuration \("dispatcher":customDispatcher\).

## How to use telemetry JS library

Download the telemetry-sdk npm module from [here](https://www.npmjs.com/package/@project-sunbird/telemetry-sdk)

```text

npm i @project-sunbird/telemetry-sdk
```

**Example:**

```text

$t = require('@project-sunbird/telemetry-sdk');   
$t.start(config, contentId, contentVer,data, options);
```

To use the telemetry JS libraries, add the following to your HTML/application. The file path is a relative path, for example; assets/js to the associated files within the html content.

```text

<!-- External Libraries -->
  <script src="[relative_path]/jquery.min.js"></script>

  <!-- Telemetry JS library -->
  <script src="[relative_path]/telemetry.min.js"></script>
  <script src="[relative_path]/auth-token-generator.min.js"></script>
  <script>
    function init() {
          // Generate auth token
          // Key: Partner generated key
          // secret: partner secret value 
          let token = AuthTokenGenerate.generate(key, secret);
          config.authToken = token;
          let startEdata = {};
          let options = {};
          $t.start(config, "content_id, "contetn_ver", startEdata, options );
      }
  init()
  </script>
```

## Telemetry API methods

Every API method has an associated event. The following API methods log details of the associated telemetry event.

* [Start](jslibrary.md#start) - This method initializes capture of telemetric data associated to the start of user action
* [Impression](jslibrary.md#impression) - This method is used to capture telemetry for user visits to a specific page.
* [Interact](jslibrary.md#interact) - This method is used to capture user interactions on a page. For example, search, click, preview, move, resize, configure
* [Assess ](jslibrary.md#access)- This method is used to capture user assessments that happen while playing content.
* [Response](jslibrary.md#response) - This method is used to capture user responses. For example; response to a poll, calendar event or a question.
* [Interrupt](jslibrary.md#interrupt) - This method is used to capture interrupts triggered during user activity. For example; mobile app sent to background, call on the mobile, etc.
* [End](jslibrary.md#end) - This method is used to capture closure after all the activities are completed
* [Feedback](jslibrary.md#feedback) - This method is used to capture user feedback
* [Share](jslibrary.md#share) - This method is used to capture everything associated with sharing. For example; Share content, telemetry data, link, file etc.
* [Audit](jslibrary.md#audit) - This method is used when an object is changed to know previous and current state. This includes lifecycle changes as well.
* [Error](jslibrary.md#error) - This method is used to capture when users face an error
* [Heartbeat](jslibrary.md#heartbeat) - This method is used to know is process is running or not.
* [Log](jslibrary.md#log) - This method is used to capture generic logging of events. For example; capturing logs for API calls, service calls, app updates etc.
* [Search](jslibrary.md#search) - This method is used to capture the search state i.e. when search is triggered for content, item, assets etc.
* [Metrics](jslibrary.md#metrics) - Service business metrics \(also accessible via health API\)
* [Summary](jslibrary.md#summary) - Summary event
* [Exdata](jslibrary.md#exdata) - This method is used as a generic wrapper event to capture encrypted or serialized data

### Start

This API is used to log telemetry when users view content or initiate game play

```text

start: function(config, contentId, contentVer, data, options) { }
```

Request Arguments:

```text

let config = Object; // Telemetry Configurations
let contentId = String; //Required. Id of the content
let contentVer = String; //Required. Version of the content. Defaults to "1.0"
let data = { // Required. event data
    "type": String, //Required.  app, session, editor, player, workflow, assessment
    "mode": "", //Required. mode of preview: preview, edit or play 
    "stageid": "" //Required. stage id where the play has been initiated
};
let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};

```

### Impression

This API is used to log telemetry when users visit a specific page.

```text

impression: function(data, options) { }
```

Request Arguments:

```text


let data = { // Required
    "type": String, //Required. Impression type (list, detail, view, edit, workflow, search)
    "subtype": String, //Optional. Additional subtype. "Paginate", "Scroll"
    "pageid": String, //Required.  Unique page id
    "itype": "", // type of interaction - SWIPE, SCRUB (fast forward using page thumbnails) or AUTO
    "stageto": "" // game level, stage of page id to which the navigation was done
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Interact

This API is used to log telemetry of user interactions on the page. For example, search, click, preview, move, resize, configure

```text

interact: function(data, options) { }
```

Request Arguments:

```text


let data = { // Required
    "type": "", // Required. Type of interaction TOUCH,DRAG,DROP,PINCH,ZOOM,SHAKE,ROTATE,SPEAK,LISTEN,WRITE,DRAW,START,ENDCHOOSE,ACTIVATE,SHOW,HIDE,SCROLL,HEARTBEAT,OTHER
    "subtype": "", // Optional. Additional types for a global type. For ex: for an audio the type is LISTEN and thesubtype can be one of PLAY,PAUSE,STOP,RESUME,END
    "id": "", // Required. Resource (button, screen, page, etc) id on which the interaction happened - use systemidentifiers when reporting device events
    "pageid": "", // Optional. Stage or page id on which the event happened
    "extra": { // Optional. Extra attributes for an interaction
        "pos": [{ "x": , "y": , "z": }], // Array of positional attributes. For ex: Drag and Drop has two positional attributes. One where the drag has started and the drop point
        "values": [], // Array of values, e.g. for timestamp of audio interactions
        "tid": "", // When interaction is between multiple resources, (e.g. drag and drop) - target resource id
        "uri": "" // Unique external resource identifier if any (for recorded voice, image, etc.)
    }
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
}; 
```

### Assess

This API is used to log telemetry of assessments that have occured when the user is viewing content

```text

assess: function(data, options) { }
```

Request Arguments:

```text

let QUESTION = {
    "id": "", // unique assessment question id. its an required property.
    "maxscore", // user defined score to this assessment/question.
    "exlength": , // expected time (decimal number) in seconds that ideally child should take
    "params": [ // Array of parameter tuples
        { "id": "value" } // for ex: if var1 is substituted with 5 apples the parameter is {"var1":"5"}
    ],
    "uri": "", // Unique external resource identifier if any (for recorded voice, image, etc.)
    "desc": "short description",
    "title": "title",
    "mmc": [], // User defined missing micros concepts
    "mc": [] // micro concepts list
}

let data = { //Required
    "item": QUESTION, // Required. Question Data
    "pass": "", // Required. Yes, No. This is case-sensitive. default value: No.
    "score": "", // Required. Evaluated score (Integer or decimal) on answer(between 0 to 1), default is 1 if pass=YES or 0 if pass=NO. 
    "resvalues": [{ "id": "value" }], // Required. Array of key-value pairs that represent child answer (result of this assessment)
    "duration": "" // Required. time taken (decimal number) for this assessment in seconds
};
```

```text

 let options = { //Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Response

This API is used to log telemetry of user response. For example; Responded to assessments.

```text

response: function(data, options) { }
```

Request Arguments:

```text

let TARGET = {
    "id": "", // Required. unique id for the target
    "ver": "", // Required. version of the target
    "type": "", // Required. Type of the target
    "parent": {
        "id": "", // Optional. parent id of the object
        "type": "" // Optional. parent type of the object. Required if parentid is present.
    }
};

let data = { // Required
    "target": TARGET, // Required. Target of the response
    "qid": "", // Required. Unique assessment/question id
    "type": "", // Required. Type of response. CHOOSE, DRAG, SELECT, MATCH, INPUT, SPEAK, WRITE
    "values": [{ "key": "value" }] // Required. Array of response tuples. For ex: if lhs option1 is matched with rhs optionN - [{"lhs":"option1"}, {"rhs":"optionN"}]
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Interrupt

This API is used to log telemetry for any interruptions that have occurred when a user is viewing content or playing games. For example; screen lock, incoming call, etc.

```text

interrupt: function(data, options) { }
```

Request Arguments:

```text

let data = { //Required
    "type": "", // Required. Type of interuption
    "pageid": "", // Optional. Current Stage/Page unique id on which interuption occured
    "eventid": "" // Optional. unique event ID
};
```

```text

 let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Feedback

This API is used to log telemetry of feedback provided by the user.

```text

// To log content start/play event
feedback: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "contentId": "", // Required. Id of the content
    "rating": 3, // Optional. Numeric score (+1 for like, -1 for dislike, or 4.5 stars given in a rating)
    "comments": "User entered feedback" // Optional. Text feedback (if any)
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Share

This API is used to log telemetry when a user shares any content with other users.

```text

// To log content start/play event
share: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "dir": "", // In/Out
    "type": "", // File/Link/Message
    "items": [{ // Required. array of items shared
        "obj": {
            "id": "",
            "type": "",
            "ver": ""
        },
        "params": [
            { "key": "value" }
        ],
        "origin": { // Origin of the share file/link/content
            "id": "", // Origin id
            "type": "" // Origin type
        },
        "to": {
            "id": "",
            "type": ""
        }
    }]
};
```

```text

 let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};

```

### Audit

This API is used to log telemetry when an object is changed. This includes life-cycle changes as well.

```text

audit: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "edata": {
        "props": [""], // Updated properties
        "state": "", // Optional. Current state
        "prevstate": "" // Optional. Previous state
    }
};
```

```text

  let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Error

This API is used to log telemetry of any error that has occurred when a user is viewing content or playing games.

```text

error: function(error, options) { }
```

Request Arguments:

```text

let error = { // Required
    "err": "", // Required. Error code
    "errtype": "", // Required. Error type classification - "SYSTEM", "MOBILEAPP", "CONTENT"
    "stacktrace": "", // Required. Detailed error data/stack trace
};
```

```text

 let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Heartbeat

This API is used to log telemetry for heartbeat event to denote that the process is running.

```text

heartbeat: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "edata": {}
}
```

```text

 let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};

```

### Log

This API is used to log telemetry of generic log events. For example; API calls, service calls, app updates, etc.

```text

log: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "type": "", // Required. Type of log (system, process, api_access, api_call, job, app_update etc)
    "level": "", // Required. Level of the log. TRACE, DEBUG, INFO, WARN, ERROR, FATAL
    "message": "", // Required. Log message
    "params": [{ "key": "value" }] // Optional. Additional params in the log message
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Search

This API is used to log telemetry when a user triggers a search for any content, item or asset

```text

search: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "type": "", // Required. content, assessment, asset 
    "query": "", // Required. Search query string 
    "filters": {}, // Optional. Additional filters
    "sort": {}, // Optional. Additional sort parameters
    "correlationid": "", // Optional. Server generated correlation id (for mobile app's telemetry)
    "size": 333, // Required. Number of search results
    "topn": [{}] // Required. top N (configurable) results with their score
};
```

```text

let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
}; 
```

### Metrics

This API is used to log telemetry for service business metrics \(also accessible via health API\).

```text

metrics: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "edata": {
        "metric1": Int,
        "metric2": Int
            /// more metrics, each is a key value
    }
};
```

```text

 let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Summary

This API is used to log telemetry summary event

```text

summary: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "edata": {
        "type": "", // Required. Type of summary. Free text. "session", "app", "tool" etc
        "mode": "", // Optional.
        "starttime": Long, // Required. Epoch Timestamp of app start. Retrieved from first event.
        "endtime": Long, // Required. Epoch Timestamp of app end. Retrieved from last event.
        "timespent": Double, // Required. Total time spent by visitor on app in seconds excluding idle time.
        "pageviews": Long, // Required. Total page views per session(count of CP_IMPRESSION)
        "interactions": Long, // Required. Count of interact events
        "envsummary": [{ // Optional
            "env": String, // High level env within the app (content, domain, resources, community)
            "timespent": Double, // Time spent per env
            "visits": Long // count of times the environment has been visited
        }],
        "eventssummary": [{ // Optional
            "id": String, // event id such as CE_START, CE_END, CP_INTERACT etc.
            "count": Long // Count of events.
        }],
        "pagesummary": [{ // Optional
            "id": String, // Page id
            "type": String, // type of page - view/edit
            "env": String, // env of page
            "timespent": Double, // Time taken per page
            "visits": Long // Number of times each page was visited
        }]
    }
};
```

```text

  let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### Exdata

This API is used to log telemetry for external data, while playing content

```text

exdata: function(data, options) { }
```

Request Arguments:

```text

let data = {
    "type":"" - Free flowing text.For ex: partnerdata,xapi etc
   ....Serialized data(can be either encrypted / encoded / stringified)

};
```

```text

  let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### End

This API is used to log telemetry while the user is closing or exiting the content or game

```text

end: function(data, options) { }
```

Request Arguments:

```text

let data = { // Required
    "contentId": "", // Required. Id of the content
    "type": "", // Required. app, session, editor, player, workflow, assessment
    "duration": "", // Required. Total duration from start to end in seconds
    "pageid": "", // Optional. Page/Stage id where the end has happened.
    "summary": [{ "key": "value" }] // Optional. Summary of actions done between start and end. For ex: "progress" for player session, "nodesModified" for collection editor
};
```

```text

  let options = { // Optional
    context: {}, // To override the existing context
    object: {}, // To override the existing object
    actor: {}, // To override the existing actor
    tags: {}, // To override the existing tags
    runningEnv: "server" // It can be either client or server
};
```

### ResetContext

This is used to reset the current context value with new context object.

```text

 @param {context} Object    - If context is undefined then library is reset to previous event context value.
 $t.resetContext(context) 
```

### ResetObject

Which is used reset the current object value with new obj

```text

 @param {obj} Object      - If the Object is undefined then library is reset to previous event object value.
 $t.resetObject(obj) 
```

### ResetActor

Which is used reset the current actor value with new actor

```text

 @param {actor} Object    - If the actor is undefined then library is reset to previous event actor value.
 $t.resetActor(actor) 
```

### ResetTags

Which is used to reset the current tag's value with new tag's

```text

 @param {tags} Array      - If tags are undefined then library is reset to previous event tags value.
 $t.resetTags(tags) 
```

## ChangeLog

**Jan 2018**

* For the `start` event, Changed the both `contentId` and `contentVer` to an optional parameters from mandtory.
* Decoupling of the both init and start methods.
* Introduced new initialize method, Where user can initialize the telemetry without calling `start` event. [More details](jslibrary.md#initialize)
* Introduced new context parameter in all telemetry event methods, Where user can easily update the context value for each event.
* Introduced `resetContext` method, Which is used to reset the context to new context value/global context. [More details](jslibrary.md#resetcontext)
* Introduced `resetObject` method, Which is used to reset the current object value. [More details](jslibrary.md#resetobject)
* Introduced `resetTags` method, Which is used to reset the current tags value. [More details](jslibrary.md#resettags)
* Introduced `resetActor` method, Which is used to reset the current actor value. [More details](jslibrary.md#resetactor)
* Previously if the user invokes an end event then the user must and should invoke start event to initialize the telemetry. but in the updated on no need to invoke start event because telemetry is initialized globally.
* Bug fixes

