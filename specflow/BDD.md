# BDD (Behavior Driven Development)

## What is BDD

BDD is a process consisting of three practices or phases, the phases are executed in a specific order

* Discovery - structured and collaborative activity to create examples and rules that aid understanding and remove ambiguity.
* Formulation - Converts the examples and rules from discovery into business readable scenarios.
* Automation - executable tests are created from the scenarios.

## What BDD is not

Just the automation part

## Discovery

A structured and collaborative activity that uses examples to uncover ambiguity and misunderstanding.

We "discover" by having "structured conversations" about changes/stories/features during "requirement workshops".

Requirement workshops should be attended by at least one person from each for the following groups:

* Testers/QA
* Developers
* Business analysts
* Product Managers/Owners
* Someone from the business/the requester

Requirement workshops are sometimes refereed to as the three amigos as the minimum amount of people needed is 3, one tester/qa, one developer and one person with business insight into the change.

### Structured conversations

Structured conversations are facilitated exchanges of ideas

* Collaborative - all attendees participate actively
* Diverse perspectives - at least the three amigos - (one tester/qa, one developer and one person with business insight into the change).
* Short - regular so feedback loops are fast ~30 mins.
* Progressive focus - Capture the progress in real time, allowing the discussion to move fast.
* consensus - agreed concrete examples are the measure of success

## Formulation

Converts the examples from the Discovery stage into business readable scenarios.  Review of these scenarios deliver confidence that the development team have understood the "requirements"

## Automation

Code is written that turns the scenarios, created in the formulation stage, into executable tests.

These tests serve a number of benefits

* When a test passes the development team can be confident they have delivered what was agreed
* Gives the development team a safety net when code needs to be changed.
* The tests form a living document of how the system should behave, readable by both developer and "The Business"
* Document is up to date and source controlled

BDD is commonly seen as a test automation tool, this is wrong. although BDD style tests can be written and executed

## Example Mapping

Example mapping is a exercise used for discovery. simmar to impact and story mapping we use colored post-its or index cards to express the following:

* Story - Yellow cards - This describes the change, outcome required and/or value
* Example - Green - illustrates concrete behavior of the system
* Rules/Requirements/Acceptance Criteria - Blue - use these to group the examples into logical groups
* Questions/Assumptions - Red - Things we don't know or need to check and will not waste time on during the meeting.

You should not use the given/when/then format when creating examples.

Give Examples a title (Friends naming)


If a story comes with a set of rules, each rule should be discussed and examples created for it, illistating the rule. 