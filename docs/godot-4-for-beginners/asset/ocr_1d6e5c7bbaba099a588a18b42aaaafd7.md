# Learn by example

There is not a single *correct* way to write a design document. A helpful way to get started is to study examples from real-world games. The **Game Docs** website (https://gamedocs.org/documents/) offers a curated collection of design documents from commercial releases. Explore these to see how others structure their ideas and adapt what works for your own project.

Now that you understand the key principles behind effective documentation, let’s take a closer look at what goes into creating a GDD.

# Exploring the Game Design Document

By now, we know that the GDD is a highly descriptive, living software design document for a video game. For developers using Godot (or any game engine), a well-prepared GDD helps streamline the development process by clearly outlining gameplay systems, scene structures, and script logic, making it easier to translate ideas into Godot’s node-based architecture and GDScript.

The first page or home page of the GDD should be a high-level overview of the game concept, which would give you the main idea behind the game. This is also sometimes referred to as a *pitch* **document**. In the following sections, we’ll look at the sections to include on this page.

# Title: what will you call your game?

The *title* is often a working title, which is subject to change as the game evolves. The title should represent the main idea of the game, so it is worth spending some time thinking about this.

As an example, our working title could be *SILENT STATION*. Let’s develop this in the following subsections.

# Team: who will build or develop the game?

List the people working on the game and what exactly they are responsible for. Don’t worry about designations; concentrate on exactly which aspects of the project they will make functional.

# Status: what is the status of the project?

This is best described using a traffic light system or a kanban board. Create color-themed headings: **TO DO** *(yellow)*, **DOING** *(orange)*, **DONE** *(green)*, and **BLOCKED** *(red)*. List the features of the game under each of these headings. Anything blocking the progress of the game or any major issues should fall under the red category. An example of this is shown in *Figure 11.1:*