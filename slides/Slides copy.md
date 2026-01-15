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

---

<!-- @@@ Sponsors @@@ -->

<!-- _footer: "" -->
<!-- _paginate: skip -->

<!-- ![height:550px](./images/conferences/2025-Atlanta-Dev-Con-Sponsors.png) -->

## **TODO** Sponsors Slide?

---

<!-- @@@ Audience Questions @@@ -->

<!-- _class: talk_topics -->

### Audience Polls

* How many have written a mobile app before?
* How many have published an app to an app store?
* How many used the M-V-VM pattern in their app?
* How many have separate API libraries in their app?

---

<!-- @@@ Topics @@@ -->

<!-- _class: talk_topics -->

### **TODO** Topics

- Separating API consumption into testable libraries
- Testing the translation of API data into app Models
- Testing use of your app Models by ViewModels
- Testing your API library for fault-tolerance
- Testing your API library for unusual responses

---

<!-- @@@ Section 1 Title @@@ -->

<!-- _class: section_title -->

## Part 1

* ### Imagine your house was an app ...

---

<!-- @@@ House - User @@@ -->

<!-- _class: details -->

![bg right:55%](./images/isometric-house-profile-concept/17395.jpg)

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
    <a href="https://www.freepik.com/free-vector/isometric-house-profile-concept_4278104.htm">
    Image by macrovector</a> on Freepik
</div>

---

<!-- @@@ House - Grid @@@ -->

<!-- _class: details -->

![bg right:42%](./images/plumbing-problems-solution-isometric-infographic-poster/17767.jpg)

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
    <a href="https://www.freepik.com/free-vector/plumbing-problems-solution-isometric-infographic-poster_4283915.htm">
    Image by macrovector</a> on Freepik
    <br />
    <i>[Equivalent energy image not available]</i>
</div>

---

<!-- @@@ House - Solar @@@ -->

<!-- _class: details -->

![bg right:48%](./images/smart-home-energy-generation-isometric-poster/16696.jpg)

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
    <a href="https://www.freepik.com/free-vector/isometric-modern-house_1086482.htm">
    Image by macrovector</a> on Freepik
</div>

---

<!-- @@@ Section 2 Title @@@ -->

<!-- _class: section_title -->

## Part 2

* ### Mobile is just like Web but smaller,

* ### right!?

---

<!-- @@@ Web vs Mobile - High-level @@@ -->

<!-- _class: details -->

### Web vs Mobile - High Level

![height:250px](./images/website-vs-mobile-high-level.drawio.svg)

<div class="commentary" style="padding: 20px 0px 0px;">

* Same thing, right? Problem solved! Crisis averted!
Thank you and good night!

</div>

---

<!-- @@@ Web Architecture - Simplified @@@ -->

<!-- _class: details -->

### Web Architecture - Simplified

<p/>

![width:1100px](./images/website-traditional-high-level.drawio.svg)

<div class="detail-summary">

- Server/cloud stack is the focus here, turning data into pages and layouts
- Complex layouts are generally built on top of templating frameworks
- <b>Web devs expect fat data pipes in the server/cloud stack to get large data payloads (object graphs) to choose what to filter/display in page layouts</b>

</div>

---

<!-- @@@ Mobile Architecture - Simplified @@@ -->

### Mobile Architecture - Simplified

<!-- _class: details -->

![width:1100px](./images/mobile-apps-high-level.drawio.svg)

<div class="detail-summary">

- App handles interactivity, page layouts, local caching, and API calls
- API stack delivers only data or status codes in response to app requests
- App pages tend to be focused on single tasks that call the API selectively
- <b>Mobile/API devs focus more on reliable, just-in-time data delivery</b>

</div>

---

<!-- @@@ Section 3 Title @@@ -->

<!-- _class: section_title -->

## Part 3

* ### "It's the data, stupid"

* ### **TODO** [Insert picture of James Carville]

* ### **TODO** [Insert picture of The War Room whiteboard]

---

<!-- @@@ Mobile Architecture - Real World @@@ -->

<!-- _class: details -->

## Mobile Architecture - Real-world

<div style="padding: 20px 0px;">

- ViewModels
  - Moving navigation out of the View allows ViewModels to be tested individually with frameworks like ReactiveUI (RxUI)
- Api Services
  - Individually testable to return app Models and not just Data Transfer Objects (DTOs)
- Data Service
  - Decides whether to call the local cache DB, the platform cache, an API endpoint, or a service based on the requested data and the app lifecycle

</div>

---

<!-- @@@ Mobile Architecture - Network @@@ -->

<!-- _class: details -->

## Mobile Architecture - Network

<br />

![width:1100px](./images/mobile-apps-real-world-network.drawio.svg)

---

<!-- @@@ Mobile Architecture - Solution @@@ -->

<!-- _class: details -->

## Mobile Architecture - Solution

<br />

![width:1100px](./images/mobile-apps-real-world-solution.drawio.svg)

---

<!-- @@@ Mobile Architecture - Testing @@@ -->

<!-- _class: details -->

## Mobile Architecture - Testing

<br />

![width:1100px](./images/mobile-apps-real-world-testing.drawio.svg)

---

<!-- @@@ Section 4 Title @@@ -->

<!-- _class: section_title -->

## Part 4

* ### **TODO** Title?

* ### **TODO** [Insert picture of **?**]

---

<!-- @@@ Demos @@@ -->

<!-- _class: section_title -->

## Part **X**

### Demos

---

<!-- @@@ Questions? @@@ -->

<!-- _class: section_title -->

## Part **X**

### OCC

---

<!-- @@@ Questions? @@@ -->

<!-- _class: section_title -->

## Part **X**

### Questions?
