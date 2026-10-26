# Smart India Hackathon Workshop
# Date:26/10/26
## Register Number: 212225040172
## Name: Karkie F
## Problem Title
SIH 1710: Enhancing Navigation for Railway Station Facilities and Locations
## Problem Description
Background: Railway stations are complex environments with numerous facilities and locations such as ticket counters, platforms, restrooms, food courts, and waiting areas. Passengers often face difficulties in navigating these spaces, especially in large or unfamiliar stations. Efficient and user-friendly navigation systems are crucial for improving passenger experience, reducing congestion, and ensuring timely travel connections. Description: The problem involves developing a comprehensive navigation solution for railway stations that assists passengers in locating various facilities and destinations within the station premises. This includes creating detailed maps, providing real-time directions, and integrating features such as accessibility options for individuals with disabilities. The solution should be intuitive, easy to use, and accessible via multiple platforms, including mobile devices and digital kiosks. Key challenges include updating navigation information in real-time, ensuring accuracy, and accommodating the diverse needs of all passengers. Expected Solution: The expected solution is a multi-platform navigation system that provides detailed, real-time directions to all facilities and locations within a railway station. This system should include: A mobile application with 3D interactive maps and step-by-step navigation. Digital kiosks located throughout the station with touch-screen interfaces. Voice-guided navigation for visually impaired passengers. Regular updates to reflect changes in station layout and facility locations. Integration with existing railway apps and services for seamless user experience. The solution should enhance the overall passenger experience by reducing confusion, saving time, and improving accessibility within the station.

## Problem Creater's Organization
Ministry of Railway

## Idea
StationSathi: navigation by time, not distance.

Existing station maps only tell passengers where things are. StationSathi tells them whether they can do what they need and still catch their train. The passenger enters a PNR, and every route is planned around the train’s departure time.

 Time-budget plan: “You have 18 minutes.   Washroom + water + Platform 6 takes 11 minutes. You are safe.”
 
 Crowd-aware routes: live crowd density changes route weights, so the shortest path is not always the one suggested.
 
 Auto re-route: if the platform changes, the route and voice guidance update at once, with the added walking time shown.
 
 Route profiles: wheelchair, elderly, luggage, family and visually impaired passengers each get their own routing graph.
 
 Kiosk to phone handoff: the kiosk shows the route plus a QR code, and scanning it continues the route on the phone with no app install.

## Proposed Solution / Architecture Diagram

![alt text](<StationSathi poster_page-0001.jpg>)

The system has four layers.

 Passenger layer: a mobile PWA with a 3D interactive map and step-by-step directions, touch-screen kiosks, and a voice and haptic guide (“Sound Trail”) for visually impaired passengers.

Routing brain:
 a time-budget engine that turns the PNR into minutes left
 profile routing that picks the right graph for each passenger type
 an auto re-route engine that reacts to platform changes

Data and sensing:
 a digital twin of the station as a versioned graph
 a live NTES train feed for platform, delay and coach position
 QR/NFC anchors on pillars plus phone motion sensors for location
 anonymous crowd counts

Staff map editor: 
  staff mark a closed lift or a moved stall, and a small “map patch” is pushed to all devices. This covers the requirement for regular layout updates.

The system also exposes an API and deep link, so IRCTC and other railway apps can embed “Navigate inside station” with one button.

## Use Cases

Late passenger:
gets the direct route to the platform with the time left.

Platform change: 
receives a new route and the extra time needed.

Wheelchair or elderly passenger: 
gets a step-free route, and a lift outage reroutes automatically.

Visually impaired passenger: 
uses Sound Trail with voice, spatial audio and vibration from entrance to coach.

First-time traveller: 
uses a kiosk and scans the QR code to carry the route on their phone.

Family with luggage and children: 
gets a low-crowd, stair-free path with a washroom or baby-care stop.

Station staff: 
update the map in minutes and view crowd bottlenecks.

Emergency: 
one tap shows the nearest exit, first aid and help desk.

## Technology Stack

Passenger app and kiosk: React PWA (Vite), offline-first with Service Workers and IndexedDB

Maps: GeoJSON indoor floor plans, MapLibre GL (2D), Three.js (3D)

Routing: Dijkstra / A* with dynamic edge weights

Backend: Node.js (Express) or Python FastAPI, with WebSocket for live re-routing

Database: PostgreSQL + PostGIS for map graphs, Redis for live crowd and train data

Location: QR/NFC anchor tags plus the Web Sensors API (accelerometer, gyroscope, magnetometer)

Voice and haptics: Web Speech API, offline text-to-speech, Vibration API

Crowd analytics: anonymous device and gate counts, with no personal data stored

Staff panel: React admin dashboard with a map editor and versioned map patches

Deployment: Docker, cloud hosting, CDN for map patches

## Dependencies

NTES / IRCTC APIs for live train status, platform and coach position

Official station floor plans or CAD files to build the digital twin

One-time installation of QR/NFC anchor plates on pillars and signboards

Station staff to maintain map patches when the layout changes

Station Wi-Fi or mobile data for live updates (the app works offline from cached maps)

A smartphone with a browser and motion sensors, or a kiosk

Compliance with India’s DPDP Act (anonymous crowd data only)

Approval from the railway apps team to embed the API or SDK
