# iOS 26 编程初学者指南 第十版：Swift 6、UIKit 和 Xcode 26 iOS 应用开发启动实用指南





# Contributors

# 关于作者

Ahmad Sahar 是 Tomafuw Productions 的培训师、演讲者和顾问。他是 Swift 应用开发的 Apple 认证培训师，并在过去 10 多年里开展了 iOS 移动应用开发、Flutter 移动应用开发以及如何使用 Apple 软件和硬件的培训课程。他目前还持有 App Development with Swift Associate 和 Certified User 认证。

他是马来西亚 DevCon iOS 在线社区的成员，并为该团体举办过演讲和讲座。业余时间，他喜欢搭建和编程 LEGO MINDSTORMS® 机器人。

献给我的母亲 Sharifah；我的父亲 Sahar；我的两个姐妹 Ainol Shareha 和 Ainol Shaharina；以及我深爱的妻子 Oni——感谢你们的爱与支持。

# 关于审阅者

**Ian Lockett** 是英国的 iOS 开发者。拥有超过 20 年软件行业经验，其中过去 15 年为各类客户和行业开发应用，从小型初创公司到大型企业。Ian 以注重细节和交付高质量软件为傲。Ian 还曾担任 “Hacking with Swift Live” 大会的志愿者联合组织者。

# 第 1 部分：Swift

Chapter 1: Hello, Xcode                                                       3

Technical requirements ............................................................................... 4

Download and install Xcode from the App Store ........................................................ 5

Exploring the Xcode user interface ................................................................... 12

Run your app in Simulator ............................................................................. 14

The Build section • 17

Run your app on an iOS device ....................................................................... 18

Trusting the Developer App certificate on your iOS device • 26

Summary ................................................................................................... 28

Chapter 2: Simple Values and Types                                         29

Technical requirements ............................................................................... 30

Swift playgrounds ..................................................................................... 30

Change fonts and colors • 34

Run playground code • 35

# Loops ......................................................... 65

for-In loops • 66

while loops • 67

repeat-while loops • 68

# Summary ......................................................... 69

# Chapter 5: Collection Types 71

# Technical requirements ......................................................... 71

# Arrays ......................................................... 72

Create an array • 73

Check the number of elements in an array • 73

Add a new element to an array • 74

Access an array element • 75

Assign a new value to a specified index • 75

Remove an element from an array • 75

Iterate over an array • 76

# Dictionaries ......................................................... 76

Create a dictionary • 77

Check the number of elements in a dictionary • 78

Add a new element to a dictionary • 78

Access a dictionary element • 79

Assign a new value to an existing key • 79

Remove an element from a dictionary • 79

Iterate over a dictionary • 80

# Sets ......................................................... 81

Create a set • 81

Check the number of elements in a set • 82

Add a new element to a set • 82

Check whether a set contains an element • 82

# Modify the Add New Journal Entry screen .................................................... 231

+   Add a custom view to the New Entry scene • 232

+   Add a switch to the New Entry scene • 236

+   Add a text field and a text view to the New Entry scene • 240

+   Add an image view to the New Entry scene • 244

+   Embed user interface elements in a stack view • 246

# Modify the Journal Entry Detail screen .................................................... 251

+   Configure the number and size of static table view cells • 253

+   Add user interface elements to static table view cells • 256

# Summary ............................................................... 263

# 第 3 部分：代码

# Chapter 14: MVC and Table Views 267

# Technical requirements ........................................................ 268

# The MVC design pattern ........................................................ 268

+   View controllers • 269

# Table views ............................................................... 270

+   Adopt the UITableViewDataSource protocol • 272

+   Adopt the UITableViewDelegate protocol • 276

+   Create a TableViewExampleController instance • 278

# Revisit the Journal List screen .................................................... 285

# Summary ............................................................... 285

# Chapter 15: Get Data into Table Views 287

# Technical requirements ........................................................ 288

# Model objects ............................................................... 288

# Create a class to represent a journal entry ............................................ 290

# Create sample data ........................................................... 301

# Display data in a table view ......................................................... 303

+   - Create a custom UITableViewCell subclass • 303
+   - Connect the outlets in journalCell • 307
+   - Update the data source methods in JournalListViewController • 309

# Summary ......................................................... 314

# Chapter 16: Pass Data Between View Controllers 315

# Technical requirements ......................................................... 316

# Pass data from the Add New Journal Entry screen to the Journal List screen ................... 316

+   - Create the AddJournalEntryViewController class • 317
+   - Connect the UI elements to the AddJournalEntryViewController class • 319
+   - Create a JournalEntry instance from user input • 321
+   - Update the table view with a new journal entry • 322

# Remove rows from a table view ......................................................... 325

# Text field and text view delegate methods ......................................................... 327

# Pass data from the Journal List screen to the Journal Entry Detail screen ................... 333

+   - Create the JournalEntryDetailViewController class • 333
+   - Connect the UI elements to the JournalEntryDetailViewController class • 334
+   - Display the details of a journal entry • 337
+   - Modify JournalEntryDetailViewController to display selected journal entry details • 340

# Summary ......................................................... 342

# Chapter 17: Core Location and MapKit 343

# Technical requirements ......................................................... 344

# Get your device location using the Core Location framework ................................... 344

+   - Modify the AddJournalEntryViewController class • 345
+   - Modify the Info.plist file • 354
+   - Create the MapViewController class • 357

# Update the JournalEntry class to conform to the MKAnnotation protocol ................... 363





## 社交媒体运营技巧分享

## Chapter 27: Test and Submit Your App to the App Store 549

Technical requirements ......................................................... 550

Get an Apple Developer account ......................................................... 550

About your Apple Developer account ......................................................... 551

- Generate a certificate signing request • 552
- Create development and distribution certificates • 553
- Registering an App ID • 556
- Register your devices • 558
- Create provisioning profiles • 559

Submit your app to the App Store ......................................................... 563

- Create icons for your app • 564
- Take screenshots for your app • 565
- Create an App Store listing • 566
- Create an archive build • 568
- Complete the information in App Store Connect • 572

Test your app ......................................................... 578

- Test your app internally • 578
- Test your app externally • 581

Summary ......................................................... 586

## Chapter 28: Unlock Your Exclusive Benefits 589

Other Books You May Enjoy 595

Index 599

## Preface

Welcome to *iOS 26 Programming for Beginners*. This book is the tenth edition of the *iOS Programming for Beginners* series, and has been fully updated for iOS 26, macOS Tahoe 26, and Xcode 26.

In this book, you will build a journal app called JRNL. You will start off by exploring Xcode, Apple’s programming environment, also known as its **Integrated Development Environment (IDE)**. Next, you will start learning the foundations of Swift, the programming language used in iOS apps, and see how it is used to accomplish common programming tasks.

Once you have a solid foundation of using Swift, you will start creating the user interface of the JRNL app. During this process, you will work with storyboards and connect your app’s scenes together using segues.

With your user interface complete, you will then add code to implement your app’s functionality. To start, you’ll learn how to display data using a table view. Next, you’ll learn how to add data to your app, and how to pass data between view controllers. After that, you’ll learn how to determine your device location and display annotations on a map. You’ll then learn how to persist app data using JSON files, create custom views, and add photos from the camera or photo library. Finally, you’ll make your app work on devices with larger screens, such as an iPad or Mac, by implementing a collection view in place of a table view.

You now have a complete app, but how about adding the latest iOS 26 features? You’ll start by learning how to update your app’s user interface to Apple’s new design language across all its platforms. Next, you will learn how to create cool icons using Icon Composer. After that, you’ll learn how to implement Apple Intelligence features and the Foundation Models framework into your apps.

Finally, you’ll learn how to test your app with internal and external testers and get it into the App Store.

## Who this book is for

This book is tailored for individuals with minimal coding experience who are new to the world of Swift and iOS app development. A basic understanding of programming concepts is recommended.

## What this book covers

*Chapter 1, Hello, Xcode,* takes you through a tour of Xcode and talks about all the different parts that you will use throughout the book.

*Chapter 2, Simple Values and Types,* deals with how values and types are implemented by the Swift language.

*Chapter 3, Conditionals and Optionals,* shows how if and switch statements are implemented, and how to implement variables that may or may not have a value.

*Chapter 4, Range Operators and Loops,* shows how to work with ranges and the different ways loops are implemented in Swift.

*Chapter 5, Collection Types,* covers the common collection types, which are arrays, dictionaries, and sets.

*Chapter 6, Functions and Closures,* covers how you can group instructions together using functions and closures.

*Chapter 7, Classes, Structures, and Enumerations,* talks about how complex objects containing state and behavior are represented in Swift.

*Chapter 8, Protocols, Extensions, and Error Handling,* talks about creating protocols that complex data types can adopt, extending the capabilities of existing types, and how to handle errors in your code.

*Chapter 9, Swift Concurrency,* introduces you to the concepts of parallel and asynchronous programming, and shows you how you can implement them in your app.

*Chapter 10, Set Up the User Interface,* deals with creating the JRNL app and setting up the initial screen that the users will see.

*Chapter 11, Build the User Interface,* covers setting up the main screen for the JRNL app.

*Chapter 12, Complete the User Interface,* covers setting up the remaining screens for the JRNL app.

*Chapter 13, Modify App Screens,* is about configuring each screen of the app in a storyboard.

To complete all the exercises in this book, you will need the following:

- A Mac computer running macOS 15.0 Sequoia, macOS 26 Tahoe, or later
- Xcode 26.0 or later

To check whether your Mac supports macOS Tahoe 26, use this link: https://www.apple.com/my/os/macos/. If your Mac is supported, you can update macOS using **Software Update** in **System Settings**.

To get the latest version of Xcode, you can download it from the Apple App Store. Most of the exercises can be completed without an Apple Developer account and use the iOS Simulator. If you wish to test the app you are developing on an actual iOS device, you will need a free or paid Apple Developer account.

The following chapter requires a paid Apple Developer account: *Chapter 27, Test and Submit Your App to the App Store*. Instructions on how to get a paid Apple Developer account are included.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E. If there’s an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Code in Action

Visit the following link to check out videos of the code being run:

https://www.youtube.com/playlist?list=PLeLCvrwLe1849hPVsrGC2DkOaPZUlVZAT

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://packt.link/gbp/9781806023936.

## Conventions used

There are several text conventions used throughout this book.

## How to Unlock

Scan the QR code (or go to packtpub.com/unlock).
Search for this book by name, confirm the edition,
and then follow the steps on the page.

**Note:** Keep your invoice handy. Purchases made directly
from Packt don’t require one.

![](img/0972ba626734a3986e095c788777818e_24_0.png)
