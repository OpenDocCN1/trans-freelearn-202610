
# 5

# Serving Static Files 211

Technical requirements 213

Static file serving 213

Introduction to Static Files Finder 214

Static file finders – use during a request 215

AppDirectoriesFinder 215

Static file namespacing 216

Exercise 5.01 – serving a file from an app directory 218

# Generating static URLs with the static template tag 222

Exercise 5.02 – using the static template tag 226

# FileSystemFinder 229

Exercise 5.03 – serving from a project static directory 230

# Static file finders – use during collectstatic 233

Exercise 5.04 – collecting static files for production 235

# STATICFILES_DIRS prefixed mode 236

# The findstatic command 239

Exercise 5.05 – finding files using findstatic 240

# Serving the latest files (for cache invalidation) 243

Exercise 5.06 – exploring the ManifestFilesStorage storage engine 245

# Custom storage engines 249

Activity 5.01 – adding a Reviews logo 252

Activity 5.02 – CSS enhancements 254

Activity 5.03 – adding a global logo 256

# Summary 258

# 6

# Forms 259

Technical requirements 259

# What is a form? 260

The form element 262

Types of inputs 263

Exercise 6.01 – building a form in HTML 264

Form security with Cross-Site Request Forgery Protection — 272

Accessing data in the View — 276

Exercise 6.02 – working with POST data in a view — 276

Choosing between GET or POST — 280

Why use GET when we can put parameters in the URL mappings? — 282

The Django Forms library — 283

Defining a form — 284

Rendering a form in a template — 295

Exercise 6.03 – building and rendering a Django form — 300

Validating forms and retrieving Python values — 305

Exercise 6.04 – validating forms in a view — 308

Built-in field validation — 311

Exercise 6.05 – adding extra field validation — 312

Activity 1 – Book Search — 314

Summary — 318

# 7

Advanced Form Validation and Model Forms — 319

Technical requirements — 320

Custom field validation and cleaning — 320

Custom validators — 320

Cleaning methods — 322

Multi-field validation — 323

Exercise 7.01 – custom clean and validation methods — 327

Adding placeholders and initial values — 336

Exercise 7.02 – placeholders and initial values — 338

Creating or editing Django models — 340

The ModelForm class — 342

Exercise 7.03 – creating and editing a publisher — 346

Activity 7.01 – styling and integrating the publisher form — 352

Activity 7.02 – Review Creation UI — 356

Summary — 363

# 8

Media Serving and File Uploads — 365

Technical requirements — 366

Settings for media uploads and serving — 366

Serving media files in development — 366

Exercise 8.01 – configuring media storage and serving — 367

Context processors and using MEDIA_URL in templates — 371

Exercise 8.02 – template settings and using MEDIA_URL in templates — 373

File uploads using HTML forms — 376

Working with uploaded files in a view — 378

Exercise 8.03 – file upload and download — 381

File uploads with Django forms — 385

Exercise 8.04 – file uploads with a Django form — 386

Image uploads with Django forms 391

Resizing an image with Pillow 393

Exercise 8.05 – image uploads using Django forms 394

ModelForms and file uploads 414

Exercise 8.07 – file and image uploads using ModelForm 416

Handling file saving 419

Serving uploaded (and other) files using Django 397

Storing files on model instances 398

Storing images on model instances 402

Working with FieldFile 403

Referring to media in templates 408

Activity 8.01 – image and PDF upload of books 421

Activity 8.02 – displaying the cover and sample link 425

Summary 427

# 9

Sessions and Authentication 429

Technical requirements 430

Middleware 430

Middleware modules 431

Implementing authentication views and templates 433

Exercise 9.01 – repurposing the Admin app login template 438

Enhancing templates with authentication data 450

Exercise 9.04 – toggling login and logout links in the base template 450

Activity 9.01 – authentication-based content using conditional blocks in templates 452

Sessions 454

The session engine 454

Do you need to flag cookie content? 455

Pickle or JSON storage? 456

Exercise 9.05 – examining the session key 457

Storing data in sessions 461

Exercise 9.06 – storing recently viewed books in sessions 462

Activity 9.02 – using session storage for the Book Search page 467

Password storage in Django 441

The profile page and the request.user object 441

Exercise 9.02 – adding a profile page 442

Authentication decorators and redirection 444

Exercise 9.03 – adding authentication decorators to the views 447

Summary 469

# 10

Advanced Django Admin and Customizations — 471

Technical requirements — 472

Customizing the admin site — 472

Discovering admin files in Django — 472

Django’s AdminSite class — 473

Exercise 10.01 – creating a custom admin site for Bookr — 475

Overriding the default admin site — 478

Exercise 10.02 – overriding the default admin site — 478

Customizing admin site text using AdminSite attributes — 480

Customizing admin site templates — 481

Exercise 10.03 – customizing the logout template for the Bookr admin site — 483

Adding views to the admin site — 485

Creating the view function — 485

Accessing common template variables — 486

Mapping URLs for the custom view — 486

Restricting custom views to the admin site — 487

Exercise 10.04 – adding custom views to the admin site — 488

Passing additional keys to templates using template variables — 491

Activity 10.01 – building a custom admin dashboard with a built-in search — 492

Summary — 494

# 11

Advanced Templating and Class-Based Views — 495

Technical requirements — 496

Template filters — 496

Custom template filters — 497

Creating custom template filters — 498

Implementing the custom filter function — 499

Using custom filters inside templates — 499

Exercise 11.01 – Creating a custom template filter — 500

String filters — 503

Template tags — 504

The types of template tags — 505

Simple tags — 505

Creating a simple template tag — 505

Exercise 11.02 – Creating a custom simple tag — 507

Inclusion tags — 510

Exercise 11.03 – Building a custom inclusion tag — 512

Django views — 515

Class-based views — 515

Exercise 11.04 – Creating a book catalog using a CBV — 517

CRUD operations with CBVs — 523

The Read view — 524

Activity 11.01 – Rendering details on the user profile page using inclusion tags — 528

Summary — 530

# 12

Building a REST API 531

Technical requirements 532

Simplifying the code using ViewSets 545

Understanding REST APIs 532

URL configuration using routers and Viewsets 546

Django REST framework 532

Exercise 12.04 – using ViewSets and routers 546

Installation and configuration 533

Implementing authentication 550

Functional API views 533

Token-based authentication 551

Understanding serializers 536

Exercise 12.05 – implementing token-based authentication for Bookr APIs 551

Class-based API views and generic views 539

Summary 556

Activity 12.01 – creating an API endpoint for a top contributors page 544

# 13

Generating CSV, PDF, and Other Binary Files 557

Technical requirements 558

Working with PDF files in Python 575

Working with CSV files inside Python 558

Converting web pages into PDFs 575

Working with Python's csv module 558

Exercise 13.04 – generating a PDF version of a web page in Python 575

Reading data from a CSV file 558

Playing with graphs in Python 578

Exercise 13.0 – reading a CSV file with Python 559

Integrating plotly with Django 583

Writing to CSV files using Python 562

Integrating visualizations with Django 583

Exercise 13.02 – generating a CSV file using Python's csv module 563

Exercise 13.06 – visualizing a user's reading history on the user's profile page 583

A better way to read and write CSV files 566

Activity 13.01 – exporting the books read by a user as an XLSLX file 589

Working with Excel files in Python 568

Summary 590

Binary file formats for data exports 569

Working with XLSX files using the XlsxWriter package 569

XLSX files 569

Exercise 13.03 – creating XLSX files in Python 572
