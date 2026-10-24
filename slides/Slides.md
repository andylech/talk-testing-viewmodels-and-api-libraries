---
marp: true
theme: custom
size: 16:9
footer: 'Andy Lech - Find Bugs Faster through ViewModel and API Library Testing'
paginate: true
---

<!-- Title -->

<!-- _footer: "" -->
<!-- _paginate: skip -->

<!-- _class: talk_title invert -->

# Find Bugs Faster through ViewModel and API Library Testing

<br />

## Andy Lech

<!--

 Browse:

 [Orlando Code Camp](https://www.orlandocodecamp.com/)

-->

---

<!-- Sponsors -->

<!-- _footer: "" -->
<!-- _paginate: skip -->

![height:640px](./images/conferences/Atl-Dev-Con-2026-Sponsors.png)

---

<!-- Orlando Code Camp -->

<!-- _footer: "" -->
<!-- _paginate: skip -->

![height:460px](./images/conferences/Orlando-Code%20Camp-2027-Early-Spring-1920x840.png)

Sanford / Lake Mary Campus, Sanford, FL
[https://www.orlandocodecamp.com/](https://www.orlandocodecamp.com/)

---

<!-- Topics -->

<!-- _class: talk_topics -->

## Topics

* What makes cross-platform and mobile different
* Real-world mobile design components
* Separating API consumption into testable libraries
* Testing how data flows through your ViewModels

---

<!-- Section 1 -->

<!-- _class: section_title -->

## Part 1

### Imagine your house was an app ...

---

<!-- Data Production, Consumption, and Storage -->

<!-- _class: details -->

### Perspectives on Data

<div class="columns-three">

<div class="column">

![width:360px](./images/houses/isometric-house-profile-concept/17395.jpg)

</div>

<div class="column">

![width:270px](./images/houses/plumbing-problems-solution-isometric-infographic-poster/17767.jpg)

</div>

<div class="column">

![width:360px](./images/houses/smart-home-energy-generation-isometric-poster/16696.jpg)

</div>

</div>

<div class="columns-three">

<div class="column">

#### User

</div>

<div class="column">

#### Web Site

</div>

<div class="column">

#### Mobile App

</div>

</div>

<div class="attribution">
    <a href="https://www.magnific.com/author/macrovector">
    Images by macrovector</a> on Magnific
</div>

---

<!-- House - User -->

<!-- _class: details -->

![bg right:55%](./images/houses/isometric-house-profile-concept/17395.jpg)

<div>

### Energy - User

##### *[Web Site or Mobile App]*

- Energy Consumption
  - Just Works
- Energy Production
  - Somebody Else's Problem
- Resiliency
  - Generator (Maybe?)
- <b>Focus</b>
  - <b>Living My Best Life</b>

</div>

<div class="attribution">
    <a href="https://www.magnific.com/free-vector/isometric-house-profile-concept_4278104.htm">
    Image by macrovector</a> on Magnific
</div>

---

<!-- House - Grid -->

<!-- _class: details -->

![bg right:42%](./images/houses/plumbing-problems-solution-isometric-infographic-poster/17767.jpg)

<div>

### Energy - Power Grid

##### *[Web Site]*

- User Consumption
  - Always
- Grid Production
  - Our Problem
- Resiliency
  - Our Backups Have Backups (Hopefully)
- <b>Focus</b>
  - <b>Keeping the Lights On (Literally)</b>

</div>

<div class="attribution">
    <a href="https://www.magnific.com/free-vector/plumbing-problems-solution-isometric-infographic-poster_4283915.htm">
    Image by macrovector</a> on Magnific
    <br />
    <i>[Equivalent energy image not found]</i>
</div>

---

<!-- House - Solar -->

<!-- _class: details -->

![bg right:48%](./images/houses/smart-home-energy-generation-isometric-poster/16696.jpg)

<div>

### Energy - Solar/Wind

###### *[Mobile App]*

- Energy Production
  - Local
- Grid Consumption
  - Backup
- Grid Production
  - Their Problem
- Resiliency
  - Battery, Power Grid
- <b>Focus</b>
  - <b>Smartly Using and Storing Energy</b>

</div>

<div class="attribution">
    <a href="https://www.magnific.com/free-vector/smart-home-energy-generation-isometric-poster_4188550.htm">
    Image by macrovector</a> on Magnific
</div>

---

<!-- Section 2 -->

<!-- _class: section_title -->

## Part 2

* ### Mobile is just like Web but smaller,

* ### right!?

---

<!-- Web vs Mobile - High-level -->

<!-- _class: details -->

### Web vs Mobile - High Level

![height:250px](./images/architecture/web-vs-mobile-high-level.drawio.svg)

<div class="commentary" style="padding: 20px 0px 0px;">

* Same thing, right? Problem solved! Crisis averted!
Thank you and good night!

</div>

---

<!-- Web Architecture - Components -->

<!-- _class: details -->

### Web Architecture - Components

<p />

![width:1100px](./images/architecture/web-traditional-components.drawio.svg)

<div class="detail-summary">

- Server/cloud stack is the focus here, turning data into pages and layouts
- Complex layouts are generally built on top of templating frameworks
- <b>Web devs expect fat data pipes in the server/cloud stack to get large data payloads (object graphs) to choose what to filter/display in page layouts</b>

</div>

---

<!-- Mobile Architecture - Components -->

### Mobile Architecture - Components

<!-- _class: details -->

![width:1100px](./images/architecture/mobile-components.drawio.svg)

<div class="detail-summary">

- App handles interactivity, page layouts, local caching, and API calls
- API stack delivers only data or status codes in response to app requests
- App pages tend to be focused on single tasks that call the API selectively
- <b>Mobile/API devs focus more on reliable, just-in-time data delivery</b>

</div>

---

<!-- Section 4 Title -->

<!-- _class: section_title -->

## Part 3

* ### Being of two minds about data

---

<!-- Mobile App - Platform Cache -->

### Mobile Apps - Platform Cache

<p />

<div class="columns-two">

<div class="detail-summary">

- Tokens are often created on login, cached locally, and renewed in the background

- Apps often store the most recent token in fast secure platform-level key:value cache

- If a valid token exists on launch, the app can skip directly to a home page

</div>

<div class="columns-two">

![height:450px](./images/screenshots/Amazon-Login-Page.png)

![height:450px](./images/screenshots/Amazon-Home-Page.png)

</div>

</div>

---

<style scoped>
pre {
  font-size: 24px;
}
</style>

### Mobile Apps - Database Cache

<div class="detail-summary">

- Akavache - [github.com/reactiveui/Akavache](https://github.com/reactiveui/Akavache)
  An asynchronous typed SQLite cache for cross-platform applications

</div>

```csharp
// Store an object
var user = new User { Name = "John", Email = "john@example.com" };
await CacheDatabase.UserAccount.InsertObject("current_user", user);

// Retrieve an object
var cachedUser = await CacheDatabase.UserAccount.GetObject<User>("current_user");

// Store with expiration
await CacheDatabase.LocalMachine.InsertObject("temp_data",
  someData, DateTimeOffset.Now.AddHours(1));

// Get or fetch pattern
var data =
  await CacheDatabase.LocalMachine.GetOrFetchObject("api_data",
    async () => await httpClient.GetFromJsonAsync<ApiResponse>("https://api.example.com/data"));
```

---

<!-- Section 3 Title -->

<!-- _class: section_title -->

## Part 4

* ### You got Presentation Logic on my Business Logic

* ### You got Business Logic in my Presentation Logic

---

<!-- MVVM Architecture -->

<!-- _class: details -->

## MVVM Pattern - Theory

<br />

![width:1000px](./images/architecture/MVVM%20Pattern%20-%20Wikipedia.svg)

<div class="attribution">
    <a href="https://en.wikipedia.org/wiki/File:MVVMPattern.svg">
    Image from Wikipedia</a>
</div>

<br />

---

<!-- Mobile Architecture - Real World -->

<!-- _class: details -->

## Mobile Architecture - Real-world

| Component | Architecture | Purpose |
| - | - | - |
| Page Layouts | Views | Presentation |
| Navigation Logic | ViewModels | Navigation Router |
| Code Behind | ViewModels | Presentation Logic |
| Code Behind | ViewModels | Data Manipulation Logic |
| Local Caches | Data Service | Data Source Logic |
| Data Access | API Services | Data Translation Logic |
| Data Access | Models | Internal Data Definitions |
| Data Access | Data Transfer Objects | External Data Definitions |

---

<!-- Mobile Architecture - Network -->

<!-- _class: details -->

## Mobile Architecture - Network

<br />

![width:1100px](./images/architecture/mobile-real-world-network.drawio.svg)

---

<!-- Section 4 - Golf Channel -->

<!-- _class: section_title -->

## Part 5

* ### Why?

---

<!-- Golf Channel - Old Tablet Splash Screen (Bad) -->

![height:640px](./images/golf-channel/Old-GC-Android-Tablet-Splash-Screen-Tiger-Cropped.png)

---

<!-- Golf Channel - Old Phone Splash Screen and Crashes -->

<div class="columns-two">

<div class="column">

![height:600px](./images/golf-channel/Old-GC-Android-Phone-Splash-Screen-Tiger.png)

</div>

<div>

![height:600px](./images/golf-channel/Old-GC-Android-Crashes.PNG)

</div>

</div>

---

<!-- Golf Channel - VP Pitch -->

![height:600px](./images/golf-channel/Golf-Channel-Cross-Platform-Pitch.jpg)

---

<!-- Golf Channel - New Phone Splash Screen and Crashes -->

<div class="columns-two">

<div class="column">

![height:600px](./images/golf-channel/New-GC-Android-Phone-Splash-Screen-Tiger.png)

</div>

<div>

![height:600px](./images/golf-channel/New-GC-Android-Crashes.PNG)

</div>

</div>

---

<!-- Section 5 - Testing -->

<!-- _class: section_title -->

## Part 6

* ### How?

---

<!-- Mobile Architecture - Real World -->

<!-- _class: details -->

## Mobile Architecture - Components

<div style="padding: 20px 0px;">

- ViewModels
  - Moving navigation out of the View allows ViewModels to be tested individually with frameworks like ReactiveUI (RxUI)
- Api Services
  - Individually testable to return app Models and not just Data Transfer Objects (DTOs)
- Data Service
  - Decides whether to call the local cache DB, the platform cache, an API endpoint, or a service based on the requested data and the app lifecycle

</div>

---

<!-- Mobile Architecture - Solution -->

<!-- _class: details -->

### Mobile Architecture - Solution

<br />

![width:1100px](./images/architecture/mobile-real-world-solution.drawio.svg)

---

<!-- Mobile Architecture - Testing -->

<!-- _class: details -->

### Mobile Architecture - Testing

<br />

![width:1100px](./images/architecture/mobile-real-world-testing.drawio.svg)

---

<!-- @@@ Refit @@@ -->

<!-- _class: details -->

### API Calls - Auto-generated from Interface

```csharp
public interface IGitHubApi
{
    [Get("/users/{user}")]
    Task<User> GetUser(string user,
      [Authorize("Bearer")] string token);
}

var gitHubApi = RestService.For<IGitHubApi>("https://api.github.com");

// Will add the header "Authorization: Bearer OAUTH-TOKEN}" to the request
var octocat = await gitHubApi.GetUser("octocat", "OAUTH-TOKEN");

```

---

<!-- Section 6 - Problem Details -->

<!-- _class: section_title -->

## Part 7

* ### There is another way ...

---

<!-- Problem Details - Description -->

<!-- _class: details -->

<div class="columns-two">

<div>

### Problem Details

##### *Pros*

- Extensible JSON object
- Provides context to status code
- Can report errors or warnings
- Supported in .NET without ASP.NET
- Returned by Refit's ApiException

##### *Cons*

- Buggy return type in ASP.NET
- .NET implementations follow JSON

</div>

<div>

```json
# NOTE: '\' line wrapping per RFC 8792
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "An RFC 7807 problem object",
  "type": "object",
  "properties": {
    "type": {
      "type": "string",
      "format": "uri-reference",
      "description": "A URI reference that identifies the problem type."
    },
    "title": {
      "type": "string",
      "description": "A short, human-readable summary of the problem type."
    },
    "status": {
      "type": "integer",
      "description": "The HTTP status code generated by the origin server \
for this occurrence of the problem.",
      "minimum": 100,
      "maximum": 599
    },
    "detail": {
      "type": "string",
      "description": "A human-readable explanation specific to this occurrence \
of the problem."
    },
    "instance": {
      "type": "string",
      "format": "uri-reference",
      "description": "A URI reference that identifies the specific occurrence \
of the problem. It may or may not yield further information if dereferenced."
    }
  }
}
```

</div>

</div>

###### Source: IETF RFC 9457 - Problem Details for HTTP APIs [www.rfc-editor.org/rfc/rfc9457.html](https://www.rfc-editor.org/rfc/rfc9457.html)

---

<!-- Problem Details - Purchase Order Error -->

<!-- _class: details -->

### Problem Details - Purchase Order Error

<br />

<div class="columns-two">

<div>

```json
POST /purchase HTTP/1.1
Host: store.example.com
Content-Type: application/json
Accept: application/json, application/problem+json

{
  "item": 123456,
  "quantity": 2
}
```

</div>

<div>

```json
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
 "type": "https://example.com/probs/out-of-credit",
 "title": "You do not have enough credit.",
 "detail": "Your current balance is 30, but that costs 50.",
 "instance": "/account/12345/msgs/abc",
 "balance": 30,
 "accounts": ["/account/12345",
              "/account/67890"]
}
```

</div>

</div>

---

<!-- Problem Details - Type Validation Error -->

<!-- _class: details -->

### Problem Details - Type Validation Error

<br />

<div class="columns-two">

<div>

```json
POST /details HTTP/1.1
Host: account.example.com
Content-Type: application/json
Accept: application/json, application/problem+json

{
  "age": 42.3,
  "profile": {
    "color": "yellow"
  }
}
```

</div>

<div>

```json
HTTP/1.1 422 Unprocessable Content
Content-Type: application/problem+json
Content-Language: en

{
 "type": "https://example.net/validation-error",
 "title": "Your request is not valid.",
 "errors": [
             {
               "detail": "must be a positive integer",
               "pointer": "#/age"
             },
             {
               "detail": "must be 'green', 'red' or 'blue'",
               "pointer": "#/profile/color"
             }
          ]
}
```

</div>

</div>

---

<!-- Section 7 - Questions? -->

<!-- _class: section_title -->

## Part 8

### References / Questions?

<!-- _footer: "" -->
<!-- _paginate: skip -->

---

<!-- Page - References -->

<!-- _class: references_list -->

### References

- Reactive Extensions (Rx or Rx.NET) - [reactivex.io](https://reactivex.io/)
  An API for asynchronous programming with observable streams

- ReactiveUI (RxUI) - [www.reactiveui.net](https://www.reactiveui.net/)
  A composable Model-View-ViewModel framework for all .NET platforms!

- Refit - [reactiveui.github.io/refit](https://reactiveui.github.io/refit/)
  An automatic type-safe REST library for .NET Core, Xamarin and .NET

- Akavache - [github.com/reactiveui/Akavache](https://github.com/reactiveui/Akavache)
  An asynchronous Key-Value store for cross-platform applications

- Splat - [github.com/reactiveui/splat](https://github.com/reactiveui/splat)
  A cross-platform library for service location, unit testing, and logging
