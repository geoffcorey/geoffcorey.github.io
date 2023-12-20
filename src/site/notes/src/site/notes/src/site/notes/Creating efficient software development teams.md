---
{"dg-publish":true,"permalink":"/src/site/notes/src/site/notes/creating-efficient-software-development-teams/","tags":["software","development","programming","process"]}
---



Creating efficient software development teams is a key success so your company can grow faster.  I am going to tell you how to overcome common problems software teams face when trying to rapidly add on new members and spin them up quickly so they can be productive quickly.

Writing good code is not enough.  When a new person joins the team they need to understand the software development process the team performs, the roles, the setup process,  on-boarding process and the procedures along that development journey from new feature to production deployment.  So how do you do that? Documentation.  I know, moan, groan, the code is the documentation and all the things developer like to avoid.   Well as a manager, VP, CIO/CTO you need to lead the way because if you don't, then your software team will be driving the business instead of the executives.

Agile retrospectives are notorious for sugar coated meetings that rarely improve the software development process because there is no documented software development process.   Somewhere it needs to be laid down and followed and if something doesn't work, the change needs to be documented and agreed upon.  Yep, just like a feature in software, the procedures in the software development process are also developed and improved.

So how do you document the process?   I recommend using Github and Markdown. Why not a wiki?  Because changes are saved and published before review.  With a source code management system the documentation can be branched, changed and reviewed before being published to master.

So what do I document?  I would start from the beginning and define the software development on-boarding process and what roles are needed.  Maybe the on-boarding process refers to other procedures beyond setting up a workstation and calls on the manager to grant access to repositories, VPNs, applications. Those documents talk about how to preform those grants.

Why is that necessary?  People know how to do that.  No they don't. If the manager is out and someone needs to fill in, say a VP.  Does he/she know all the systems that he needs to grant access?   If the manager leaves do you want all that knowledge to leave with him?  That goes for any of the roles the team(s) play in the process: release, build, environmental management.   This is your business and relying on tribal knowledge is slowing your company down and putting quality at risk if key players decide to leave voluntarily or not.
# Suggestions for starting?

Here are some basic routines you can start your team off on writing.
1. General Management
  - On-Boarding a new employee
  - Development workstation setup
2. Development
  - Workstation setup
  - Daily Meeting
  - Planning Meeting
  - Design Meeting
  - Backlog Management
2. Release Management
  - Writing Release Notes
  - Approval Process
  - Release to lower environment
  - Release to production
3. Environment Management
  - Setup new environment
  - Change management
  - Expanding an environment
4. Production Management
  - Runbook(s) - common errors and how to fix them
  - How to search log files
  - How to view runtime statistics

# Routine Template ID - Title

Brief description

## Prerequisite(s)

Things that have to happen first.  This can be a list and may include
links to other routines

## Step(s)

Steps to complete the tasks.  If a step is too complex, you can create another routine or have a routine for common step used by multiple routines.

## Alternate Paths

These are exception paths if certain conditions are met in the *Step(s)*
section

## References

Reference information to clarify step(s) or procedures for this Routine.  This can include security related or system documentation.
