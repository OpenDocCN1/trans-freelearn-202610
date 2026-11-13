
## The Animations Demo app – introducing SeekBar
- Laying out the animation demo
- Coding the XML animations
- Wiring up the Animations Demo app in Java

## Adding animations to Note To Self
- Defining the animations in XML
- Controlling the animations in Java
- FAQ
- Summary

## 17. Sound FX and Supporting Different Versions of Android
- Supporting multiple versions of Android
- Detecting the current Android version
- The SoundPool class
- Building SoundPool the new way and the old way
- Loading sound files into memory
- Playing a sound
- Stopping a sound
- Introducing the Spinner widget
- Making sound FX
- Laying out Sound Demo UI
- Coding the Sound Demo app
- Sound FX touches to Note To Self
- Deleting a note – introducing OnLongClick
- FAQ
- Summary

## 18. Design Patterns, Fragments, and the Real World
- Introducing the model-view-controller pattern
- The model
- The view
- The controller
- The imperfections of Note To Self revisited
- Android design guidelines
- Real-world apps
- The device detection mini app
- Configuration qualifiers
- Using configuration qualifiers – mini app
- The configuration qualifier's summary

## Fragments
- Fragments have a lifecycle too
- onCreate
- onCreateView
- onAttach and onDetach
- onStart, onPause, and onStop
- Managing Fragments with FragmentManager
- Our first working Fragment mini app
- Fragment reality check
- FAQ
- Summary

## 19. Using Multiple Fragments
- Using singletons for the model layer
- Inter-Fragment communications – interfaces revisited
- The dual-Fragment address book mini app
- Getting started
- Alias resources
- The NameAndAddress class
- Creating the communication interface
- Explaining the structure of the onListItemSelected method
- Explaining the code that starts a new Activity
- Explaining the code when the detail Fragment is already present
- What do we need to do?
- Creating the AddressDetailFragment class and layout
- Coding PortraitDetailActivity and the layout
- FAQ
- Summary

## 20. Paging and Swiping
- Building an image gallery/slider app
- Implementing the layout
- Coding the PagerAdapter class
- Coding the MainActivity class
- Running the gallery app
- Building a Fragment pager/slider app
- Coding the SimpleFragment class
- fragment_layout
- Coding the MainActivity class

### The activity_main layout
### Running the Fragment slider app
### Summary

## 21. Navigation Drawer and Where It's Snap
- Introducing the navigation drawer
- The Where it's snap app
- Preparing the resources
- Placeholder code for three Fragments
- Coding the MainActivity class and layout
- Switching between Fragments
- Setting up the drawer
- Handling the back button

### Summary

## 22. Capturing Images
- Capturing images using the camera
- The capturing images mini app
- Adding the camera permission to the manifest
- Defining a simple layout
- Coding the MainActivity class
- Running the Simple Photo app
- Where it's snap – coding the capture Fragment
- Creating the layout for the capture Fragment
- Coding the capture Fragment
- Testing the capture Fragment

### Summary

## 23. Using SQLite Databases in Our Apps
- Database 101
- What is a database?
- What is SQL?
- What is SQLite?
- The SQL syntax primer
- The SQLite example code
- Creating a table
- Inserting data into the database
- Retrieving data from the database
- Updating the database structure
- The Android SQLite API

### SQLiteOpenHelper and SQLiteDatabase
### Building and executing queries
### Database cursors
### The database mini app
### Implementing the layout
### Coding the database class
### Coding MainActivity
### Running the mini app
### Summary

## 24. Adding a Database to Where It's Snap
- The Photo class
- Handling the SQLite database
- Saving a new photo from the capture fragment
- Displaying a photo from the database
- Preparing the View layout
- Coding the ViewFragment class
- Coding the communications interface
- Coding TitlesFragment
- Coding TagsFragment
- Coding the fragment communications in MainActivity
- Running the app so far
- Summary

### Summary

## 25. Integrating Google Maps and GPS Locations
- Global Positioning System
- Where in the world – the GPS mini app
- Where in the world – implementing a simple layout
- Coding the Where in the world mini app
- Running the Where in the world mini app
- The Google Maps mini app
- Preparing to connect to Google Maps
- Acquiring an API key
- Installing the Google Play services API
- Coding the maps activity layout
- Coding MapsActivity
- Running the Simple Map app
- Summary

### Summary

## 26. Upgrading SQLite – Adding Locations and Maps

### Adding locations and maps to Where it's Snap
### Updating the database
### Adding member variables to represent location data
### Updating the database version
### Adding code in onUpgrade to upgrade the database for existing users
### Updating the database creation code in onCreate for new users
### Updating the addPhoto method to handle GPS coordinates
### Updating the Photo class
### Updating CaptureFragment
### Updating ViewFragment
### Adding location permissions
### Testing the new map feature
### Summary

## 27. Going Local – Hola!
- The Localization mini app
- Preparing the layout
- Preparing the first strings.xml file
- Making the app multilingual with the second strings.xml file
- Adding a third language
- Running the app
- Localizing the Where it's snap app
- Summary

### Summary

## 28. Threads, Touches, Drawing, and a Simple Game
- Threads
- Problems with threads
- Drawing with canvas and paint
- The Android coordinate system
- Android Canvas demo app
- Handling touches
- A simple game engine
- Building a simple game of Pong
- Adding sound files to the project
- Coding the ball
- Coding the paddle
- Coding MainActivity
- Coding SurfaceView
- Coding the update method

### Coding the draw method
### Lock the screen orientation and make it fullscreen
### Running the game
### Pong enhancements
### Summary

## 29. Publishing Apps
- Preparing to publish
- Creating an app icon
- Preparing the required resources
- Building the publishable APK file
- Publishing the app
- Marketing
- More to see
- Summary

### Summary

## 2. Module 2
### 1. Asynchronous Programming in Android
- Android software stack
- Dalvik runtime
- ART runtime
- Memory sharing and Zygote
- Android process model
- Process ranks
- Process sandboxing
- Android thread model
- The main thread
- The Application Not Responding (ANR) dialog
- Maintaining responsiveness
- Concurrency in Android
- Correctness issues in concurrent programs
- Liveness issues in concurrent programs
- Thread coordination
- Concurrent package constructs
- Executor framework
- Android primary building blocks
- Activity concurrent issues
- Manipulating the user interface
- Service concurrent issues

### Started services issues
### Bound services issues
### Service in a separate process
### Broadcast receiver concurrent issues
### Android concurrency constructs
### Summary

## 2. Performing Work with Looper, Handler, and HandlerThread
- Understanding Looper
- Understanding Handler
- Sending work to a Looper
- Scheduling work with post
- Using Handler to defer work
- Leaking implicit references
- Leaking explicit references
- Updating the UI with Handler
- Canceling a pending Runnable
- Scheduling work with send
- Cancelling pending messages
- Composition versus inheritance
- Multithreading with Handler and ThreadHandler
- Looper message dispatching debugging
- Sending messages versus posting runnables
- Applications of Handler and HandlerThread
- Summary

### Summary

## 3. Exploring the AsyncTask
- Introducing AsyncTask
- Declaring AsyncTask types
- Executing AsyncTasks
- Providing indeterministic progress feedback
- Providing deterministic progress feedback
- Canceling an AsyncTask
- AsyncTask Execution State
- Handling exceptions
- Controlling the level of concurrency
- Common AsyncTask issues
- Fragmentation issues
- Memory leaks
