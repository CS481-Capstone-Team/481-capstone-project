# CS 481 Capstone Project

Repository for our CS 481 Capstone project.

## Team

- Guilherme Colombo
- Shahina Aqdodova
- Monica Kissimina
- Zachary Chow

## Project Status

Project idea and scope are still being determined.

Repo is being set up early so the team can establish development workflow, issue tracking, and source control practices.

## Course

CS 481 - Senior Capstone I  
Fall 2026

## Proposed Projects

### Proposal 1: E2E Encrypted Collaborative Notes Application

**Name:** E2E Encrypted Collaborative Notes Application

**Problem:** Existing collaborative note-taking tools generally require users to trust the service provider with access to their readable data. Teams also need ways to collaborate, synchronize changes, recover previous versions, and experiment with changes without losing work.

**Target users:** Individuals and groups who want to create, organize, and collaboratively edit notes while keeping their readable content private from the hosting server or service provider.

**Proposed solution:** A collaborative note-taking application where all readable note data is encrypted and decrypted exclusively on client devices. The server stores and synchronizes encrypted data only. Users can maintain private notes or share notes with groups. The application will provide version history and lightweight git-like operations such as branching, merging, pull, push, and retrieval of previous versions. Real-time simultaneous collaboration would be nice as well.

**Technology stack:** TBD.

---

### Proposal 2: Infinitely Divisible Work Tree

**Name:** Infinitely Divisible Work Tree (working title)

**Problem:** Most work tracking tools use fixed levels, such as epic, story, task, and subtask. Work that does not fit those levels has to be forced into them. Progress at higher levels is usually entered by hand, so it can differ from the work beneath it. Access is typically set for a whole project, not for individual pieces of work. This product uses a single type of item that can be split without limit, calculates progress from the pieces beneath it, and sets access on each piece.

**Target users:** Large organizations with thousands of employees across many departments, such as engineering, operations, legal, and HR. This includes executives who want to see company-wide progress, managers who hand out work and track their own part, and individual employees who only need to see their own slice.

**Proposed solution:** A work tracker built on the idea that any piece of work can be split into smaller pieces of work, as many times as needed. A piece's level is just how deep it sits in the tree. Each workspace holds one tree with a single top item, and workspaces are separate from each other unless someone links them. The smallest pieces, the ones with nothing under them (leaf nodes), have a status that people set by hand, such as not started, in progress, completed, or postponed. Every larger piece works out its progress automatically from the pieces beneath it, so the numbers are always correct at every level (with manual sign-off for every node computed complete). Whoever owns a piece can split it and give the smaller pieces to people or teams. They can also allow those people to split and hand out work further, so responsibility passes down as deep as the tree goes. No one can give away a right they don't have. Access is set for each workspace, each piece, and each person or team, so someone can be limited to their own piece and what is under it. The work can be shown as a tree with progress at each level, or as a filterable list, with other views such as a board, a timeline, or a workload per person.

**Technology stack:** TBD.



---

### Proposal 3: TBD

**Name:** TBD

**Problem:** TBD

**Target users:** TBD

**Proposed solution:** TBD

**Technology stack:** TBD.

## Repository Structure

The repository structure will be defined once the project and technology stack are selected.

## Getting Started

Setup and development instructions will be added once the project environment is established.