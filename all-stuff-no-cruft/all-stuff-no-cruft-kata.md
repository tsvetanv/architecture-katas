
<img src="/all-stuff-no-cruft/resources/all-stuff-no-cruft-logo-thumbnail.jpg" />

# Kata: All Stuff, No Cruft (Conference Management System)
Reference: [nealford.com](https://nealford.com/katas/kata?id=AllStuffNoCruft) 

Conference organizer needs a management system for the conferences he runs for both speakers and attendees.

## Users
- hundreds of speakers
- dozens of event staff
- thousands of attendees

## Requirements
- attendees can access speaking schedule online, including room assignments
- speakers can manage talks (enter, edit, modify)
- attendees 'vote up/down' talks
- organizer can notify attendees of schedule changes up-to-the-minute (if attendees opt in)
- each conference (being a different subject) can be branded independently
- speaker slides are accessible online only to attendees
- evaluation system via web page, email, SMS, or phone

## Additional Context
- Conference runs across the US.
- Very small support staff.
- 'Bursty' traffic: extremely busy when conference is occurring.
- Conference organizer wants to easily 'skin' the site for different technology offerings.

***

# Implementation

## Domain
conference, staff member, attendee, speaker, talk, room, tolak vote, notification (talk, event, staff member), conference evaluation, talk slide, schedule

## API Design
| Functional Requirement | API | Description |
|----------|-----------------------------|----------|
| Get schedule  | `GET /cms/v1/conference/{confId}/schedule` | Get a schedule for concrete conference |
| Manage talks | `PUT POST /cms/v1/conference/{confId}/speaker/{speakerId}/talk` | Speaker manages talks |
|Vote talks|`POST /cms/v1/conference/{confId}/speaker/{speakerId}/talk/{talkId}/vote`|Attendees vote up/dpwn talks|
|Notification|`POST /cms/v1/conference/{confId}/organizer/notification/attendees`| Organizer notifies attendees about conference schedule changes |
| Manage conference | `PUT POST /cms/v1/conference` | Create, modify, rebrand conference |
|Get talk slides|`GET /cms/v1/conference/{confId}/speaker/{speakerId}/talk/{talkId}/slides`|Attendees gets speaker slides|
|Evaluation system|`POST /cms/v1/conference/{confId}/attendee/{attendeeId}/evaluation/questionare`|Attendee evaluates a conference|

## Architecture Characteristics (Quality Requirements)
- Elasticity
- Scalability
- Configurability

## Rough Estimations
5 conferences X 3 speakers = 15 QPS <br/>
5 conferences X 10 staff members = 50 QPS <br/>
5 conferences X 1000 attendees = 5000 QPS <br/>
Total: ~ 6000 QPS <br/>
 <br/>
 Dynamic data (schedule conference): 6000 QPS
 Static data (speakers' talk slides): 5000 QPS

 Predefined data schema => relational DB
 Infrastructure: public cloud provider (Azure, AWS) because of architecture characteristics elasticity and scalability. We will cover 2 region: US East and US West.
 
 ## TODO 1

 ## TODO 2
