---
layout: default
title: Future Work
nav_order: 6
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---
# Future Work

There are a number of new features for the app which have been brainstormed, or which we began to work on, but were not finished and can be future work.

## Dining Index Issue
**HIGH PRIORITY** 

The `Dining` page attempts to load the most relevant menu based on time of day (i.e. if it's noon, it will attempt to find the lunch menu to display) using the `calcDefaultIndex` function in `Dining.jsx`. The current function that does this can cause crashes between 10 am and 2 pm if there is only one meal being served that day, since it can accidentally return an index that is out of bounds. Fixing this should be as simply as adding a few checks to ensure that the returned value is less than the number of meals.

## Parking
A specific page which includes a `FUNowMapView` at the top with campus' parking zones demarcated on the top and all relevant links/resources for parking at campus below it. Relevant information would be largely moved from the `Links` page to here, and would include parking permit purchase portal, parking ticket appeal site, the parking department's contact information, accessibility maps, the form to request main circle access, temporary permit requests, and anything else related to parking.

Zones would need to have polylines drawn for them and the coordinate points encoded using the Google Maps polyline encoding for storage in the database

Marking specific electric charging spots and handicapped parking spots could be an additional feature.

Work on this page started, and is still present in the codebase.

## Library
### Library Room Reservation
The LibCal API that we use to collect the campus' libraries' hours seems to have the ability to reserve rooms in the library. Our API key does not currently have permissions to do this, but Scott Salzman (who manages the system and provided us with the API key) seemed open to the idea. We could have a `Carriage` with a simplified map of each floor of the main Duke Library, with some sort of layout of buttons for rooms that roughly approximate the layout of study rooms on each floor. The buttons would display the room name and if it is currently reserved, and clicking on it would show you a screen where you can reserve it some time (which defaults to being the current time block). 

The online reservation system requires the use of an email to reserve the space, so once someone has filled the information in once, we could save it locally to make future reservations easier.

### Directions to Library Number's Location
The Library stores books using the Library of Congress system, which some students find difficult to use. One feature requested of us by the public was a way to input a Library of Congress number and get directions on how to get to that shelf within the library. This would probably necessitate a way of storing the locations of all of the shelves in the library, and their start and end LoC numbers, and then calculating from the provided number where it falls in this group.

## Campus Feature Gamification 
### Benches
A map of most benches on campus exists in this [ArcGIS Online Project], and has been copied into a database table on the Furman Now! server. Several things could be done with this: a map that displays the different types of information, a selector which recommends a random bench (potentially with some specified characteristics) (this will probably have to take into account how many benches are nearby to the bench being selected and decreases the liklihood of benches in 'common bench regions' from being selected, just to make sure that people aren't being sent to the same places every time), the ability to send a 'kudos' of some kind to a specific bench (and the total number of kudos each bench has recieved can be displayed), and some sort of gamification of visiting every bench. 

The game could be several different things; people could be required to check-in to every bench by clicking on it, which saves into their phone's async storage that they have visited it; or it could be a 'fog of war' style system where only after traveling within some distance of the bench does it appear on the map; or it could be a scavenger hunt where a variety of characteristics including proximity to buildings, material types, etc. are used to make a description of a bench which can only fit one on campus, and then users have to travel to and check in at that bench.

These could help make walking around campus more fun.

### Other Campus Maps
Dr. Suresh Muthukrishnan from EES has collected a number of other maps relevant to campus, and they are stored online [here][Campus Object Map]. Of the maps present, the most relevant to me seems to be the bike rack map and the restroom/water fountains/trash cans. These could be worked into some sort of display.

One way of using some of this data - both bike racks and benches could potentially be used - is some sort of gameified race. The user could select if they are walking or running or biking, and then be given a series of points that they need to travel to in; each time they 'check in' at one point, they will be given the next one. When they arrive at the final point, they will be told how long they took. 

This could make for some sort of exercise game, and there could be some sort of leaderboard if we wanted. In addition to making sure the random selection of benches or bike racks was normalized for how many there are nearby, we would probably also want to make sure that no two successive points are both close to a parking lot so that people can simply drive between the points to cheat the system. Points should probably be generated server-side, and the route could change every day with daily leaderboards for the day's route in each of the three styles (walking, running, biking) as well as cumulative points for history of how well you have done. 

## Weather
The weather scraper is able to get humidity information, but we don't currently store it in the database, and thus cannot display it in the app. This would potentially be a good feature to include. Further, in consistency with the mission of bringing Furman information together, it might make sense to draw information on current weather from the [Furman Weather Station] (this would probably require web scraping, though Miles Dame has potentially offered to write an API for the page). It may also be nice to make clicking on the weather widget in the app direct to this page so viewers can get a fuller image of the weather.

## Hours
### Belltowwer Bookstore
The new bookstore opened after work on the app had largely concluded. Updating the scraper to pull information from the [Follett Website] would make sense, though it may be hard to parse when exactly the hours apply with the current design of the site. Contacting Follett to ask if they have an API may be worth the effort.

## Dining Menu Details & Allergy Info
Currently, dining entries do not include details (such as variations in food within stir fry at the Mongolian grill), nor do they have information about vegitarian/gluten free/shell fish/etc status. Including this information would help make the `Dining` screen a more total replacement of the website.

## Campus Directory
Contacts currently only has a limited number of contacts. It could be directed by writing a web scraper that goes through the [Directory][Campus Directory] and pulls the contact information for all staff members, which are displayed on a searchable page either within or in addition to `Contacts`. 

## Major Information
A list of all majors/minors on campus with links to their requirements and their faculty could be beneficial to students. Faculty have mentioned in the past struggling to get to their own department's major maps.

## Athletics Rosters
Including a link to the sport's roster could be beneficial, and has been requested. The link to this would be provided by the server, and could be added context menu option when an `AthleticsButton` is held down. It could additionally be the site opened by clicking on the button before a game recap link exists. 

## Improved iPad Supprt
Improving the way that the app is displayed on larger screens like iPads/tablets could make for better experience. 

## Dynamic Font Size
Font size is currently the same regardless of device type, screen size, or how the device displays the font. Creating a method of making the font larger based on the screen size may be desirable so that iPads do not have tiny text. This could probably be done by making a function in the `styling` field in the theme which takes in a font size and calculates a 'relative size' based on the screen properties.

## News
### Time Issues
Some of the scrapers do not correctly parse time zone information, leading to publication date information being saved in UTC when it should be in EST. This effectively makes the app believe the story was posted at some point in the future. This is handled by displaying that the story was "Posted Recently", which regularly is displayed for stories collected by those scrapers that have this issues, but this should be seen as a bug instead of an expected behavior. Correcting problematic news scrapers should be done at some point.

### Library Blog
The Furman Library [has a blog][Library Blog] (with an associated [RSS feed][Library RSS]) where they post various articles written by librarians. Miles Dame has requested that we add this to the News Feed, and I seen no reason to deny the request.

### Push Notifications for New Stories
Allowing users to opt-in to relevant push notifications for specific sources could be useful. These should only come out some period of time after the story is posted so that we will make sure not to spam them with too many if a source posts in bulk. We could also programmatically generate some sort of "week in review" newsletter that is sent out

### Persistent Source Filters & Settings
Some readers may only want to get news from certain sources, or be able to filter out to only see a few sources at a time, or only pick stories from certain sections of a publication. This can be accomplished in some sense by searching for the publication's name, but that is not persistant, nor does it allow multiple sources to be included simultaneously. Adding these configurations may make it more useable. It could also allow them to toggle on or off the stacking of stories from the same source, or configure push notifications once implemented, or only display student-written content.

## Admissions
Working with admissions to add features that are targeted towards new students and improving the new student experience could make the app more useful. This could include adding the [Admissions Blog] to the news feed, though this is largely aimed at incoming students rather than current ones, and may not be apt for the campus audience at large, so I would recommend doing so only with consideration.

---
[ArcGIS Online Project]: https://services1.arcgis.com/4pYHyjjOPNwIcnDB/arcgis/rest/services/Benches_On_Furman_University_Campus/FeatureServer
[Campus Object Maps]: https://furman.maps.arcgis.com/apps/View/index.html?appid=822d3f23946a422fa198dcb933962927
[Campus Directory]: https://www.furman.edu/people/staff/
[Library Blog]: https://blogs.furman.edu/library-news/
[Library RSS]: https://blogs.furman.edu/library-news/feed/
[Admissions Blog]: https://www.furman.edu/admissions-aid/admission-blog/
[Furman Weather Station]: https://www.weatherlink.com/embeddablePage/show/8aaa4bd0172f414daaf7ca82d7142383/slim
[Follett Website]: https://www.bkstr.com/furmanstore/store-hours
