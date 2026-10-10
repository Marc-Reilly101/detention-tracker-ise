# Detention Tracker Application - ISE Entrance Submission
*Technology Project Report for University of Limerick - ISE Submission*

---
The Detention Tracker Application is a web service designed to record and keep track of detentions in a school environment. It is a deployed service actively used by nine administrators.
The purpose of this README is to provide additional architectural details of the app's codebase.

---
## Tech Stack
- Vanilla JavaScript, without the use of a front-end framework such as ReactJS.
- Deployed using Netlify.
- Backend using two serverless functions *(See Section 3 in main report)*.
- Database store via three JSONBin endpoints.

## Architecture
- Heavy use made of individual objects dedicated to different areas of service (detention addition, detention removal, attendance, etc.)
  ```javascript
    /*
    Attendance Functions Module
    Manages daily detention attendance tracking with one-time submission per day.
    Attendance resets daily at midnight Coordinated Universal Time (handled by App.checkData).
    */
    const attendanceFunctions = {...}
  ```
- Numerous methods in these objects dedicated to particular tasks. Large tasks separated into multiple methods for debugging purposes.
  ```javascript
    /*
    verifyDetentionToday() - Checks if detentions are scheduled today and if
    attendance hasn't been submitted yet (prevents duplicate submissions)
    */
    verifyDetentionToday: function() {
       ...
    },
    ...
  ```
---
- The app is displayed in one singular page. Its menus are divided into different div elements called "screens" who's visibility can be toggled via the ```UI``` object's ```goTo()``` method.
  ```html
    <div id="mainscreen" class="hidden">
        <div class="header"><b>St. Colman's | Detention Book</b></div>
        <!-- webcontent classes used to establish margin sizes, and element alignment -->
        <div class="webcontent">
            ...
        </div>
    </div>
  ```
  ```javascript
    const UI = {
      currentScreen: "...",

      //The goTo method manages screen navigation and initialises form cleanup for relevant menus (profilesearchscreen, addstudentscreen, etc.)
      goTo: function(to, from) {
          var destinationScreen = to;
          //Additional menu-specific code goes here...
  
          document.getElementById(destinationScreen).style.display = "block";
          document.getElementById(from).style.display = "none";
      },
      ...
    };
  ```



## Notable Issues
The Detention Tracker App is *not* perfect. Here are some of its notable issues:
- The database infrastructure is not robust: JSONBin is a lightweight project with a small development team and outages are a possibility. As of October 2026, the app has experienced two notable outages that have disrupted admin work.
- The authentication infrastructure is imperfect: While there are safeguarding measures in place, the application's data pathway is only lightly insulated from potential attacks. Fortunately, DDoS mitigation measures are enforced by Netlify.

## Future Development
Security of the data is very important to me. Given that the app handles information about minors, there are certain requirements that need to be fulfilled. Therefore, I'm currently in the planning process for an eventual system
migration to Firebase. This will provide numerous advantages:

- Enhanced data security with Firestore Rules
- Industry-grade authentication systems and password protection
- Reliable data storage - close to no outages year-long.
- Realtime client-side updates. Will eliminate scale, locally cached copies of data.
- Will introduce scalibity to the app!

### Note on use of AI
Certain areas of the codebase were developed with *the help* of artificial intelligence. Usage was limited to where assistance was genuinely needed to solve an issue, or to suggest
ways to improve existing infrastructure. Under no circumstances was AI-generated code used in the codebase. All concepts derived from AI were tested rigorously before deployment.
The design of this application's user interface, the codebase and the application's features are exclusively my work.

---

## Bibliography

Live Resources:
- St. Colman's College Website: www.stcolmans.ie <br>
- Live Detention Tracker Application: https://stcolmansdetention.netlify.app <br>

Documentation:
- Google Cloud API Documentation: https://docs.cloud.google.com/apis/docs/overview
- JSONBin API Documentation: https://jsonbin.io/api-reference
- [Google Cloud Object Concurrency Documentation](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search-2/data-objects/using-etags-for-concurrency-control)
- Firestore Documentation: https://firebase.google.com/docs/firestore
