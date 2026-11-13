
## Using singletons for the model layer

A singleton, as the name suggests, is a class where there can only be one instance of that class. If you think about data storage, this makes sense. If you have more than one place to store the same set of data, it is possible, if not highly likely, that at some point you will attempt to store or retrieve something inconsistently.

Think about having two identical sets of folders to manage your e-mails. If you have two (or more) folders marked as urgent, how will you know where to store that urgent e-mail. And when you need to check if you have any urgent emails, you will need to check both. What if you forget and only check one?

Also, consider a shopping app, where the user browses from page to page and each page is, perhaps, a separate Fragment/Activity. If they add something to the cart on more than one page, they could end up with two separate carts instead of one with both the items in it.

When we are making straightforward apps, it is possible that we could survive without using singletons. But as our apps quickly become more complicated, it will be very easy to find our data storage solution, literally all over the place.

If it's starting to sound like singletons might be complicated, be reassured that they are quite straightforward. In software engineering, the singleton pattern is simply a design pattern that restricts the instantiation/creation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system. Note that our singleton will be accessible throughout the entire app but not to other apps.

The other thing that our singleton design will ensure is that the data persists for the entire life of the app and is available throughout the classes (including fragments) of the app.

This makes sense for us because although there are ways to communicate between classes, some of which we have already seen, if the data is fundamental to the app, it should be easily available throughout.

Let's review our requirements. We need a class that can only have one instance, but this one instance must be available to all throughout the life of the app at the same time as controlling how its data is accessed.

Take a look at this block of code for example:

```
public class Movie{
private String mTitle;
private boolean mOnLoan;
public void setTitle(String title){
mTitle = title;
}
public String getTitle(){
return mTitle;
}
public void setLoanStatus(boolean loanStatus){
mOnLoan = loanStatus;
}
public boolean getLoanStatus(){
return mLoanStatus;
}
}
```

In the previous code, we have a simple class called Movie with two private members: mTitle and mOnLoan. The two variables would hold the title of a movie and the status whether it is currently out on loan or not. This could be a useful class for an app to manage a movie rental store or perhaps a personal movie collection. The code also has two getters and two setters that are used to check and change the value of these two private member variables, which would otherwise be inaccessible from outside the class.

The class is public, so any part of our app can create instances of the class in order to manipulate data of the Movie type. The trick is how our app obtains these instances, and this is where the singleton comes in handy. Take a look at this next code block as an example:

```
public class RentalStore {
// I am instantiating an instance of myself - weird!
// This is the only RentalStore that will ever exist
// This is the only RentalStore class that will ever exist
private static RentalStore ourInstance = new RentalStore();

private ArrayList <Movie> mMovies;

// Anyone who wants my one and only instance will
// need to use this method
public static RentalStore getInstance() {
    return ourInstance;
}

// Here is the constructor and it is private
// So only this class itself can ever instantiate it
private RentalStore() {
    mMovies = new ArrayList<Movie>();
}

// Anyone can get the list of movies though
public ArrayList <Movie> getMovies() {
    return mMovies;
}
}
```

The previous code might look complicated, but when we break it down, it really isn't. First, look at the constructor (the method with the same name as the class). The RentalStore constructor method is private. Since we have provided a default constructor that is private, it might at first seem that it is now impossible to ever instantiate RentalStore.

Just before the constructor, we have a public static method called getInstance. Recall that in [Chapter 9, Object-Oriented Programming](#), we discussed that a static method can be called without an instance of the class. Look again at the getInstance method and note that its return type is RentalStore. This means that any code anywhere in our app can call the getInstance method and receive a RentalStore instance.

Now, look at the body of getInstance, and it does indeed return ourInstance, which was instantiated just after the class declaration.

This means that nobody can ever instantiate RentalStore, but everybody can get a reference to the one and only RentalStore stored in our singleton.

Furthermore, the class has a public method getMovies that returns ArrayList of all the Movie objects that have been created so that they can do what they like to the movies in it—as long as it is allowed by the getters and setters of the Movie class. This is a kind of data paradox. Complete freedom with complete control! Any code that does anything to any movie in ArrayList or adds or removes anything to/from ArrayList is guaranteed to be working on exactly the same set of data as any other part of our app that does so.

We could go on to add more features to our singleton, perhaps, to add getters and setters and to control exactly what can and can't be done to ArrayList instead of simply handing ArrayList to the calling code. This discussion has taken the concept of a singleton far enough for the purpose of the next few apps.

We can now go about using RentalStore in our code like this:

```
RentalStore myRentalShop = RentalStore.getInstance();
```

We can then get hold of the list of movies like this:

```
ArrayList <Movie> myMovies = myRentalShop.getMovies();
```

We can now do whatever we like within the constraints of the Movie class. And if later on in our code, probably in another Activity or Fragment, we have code like this, we can be confident that the code is working on exactly the same set of data as the code in the other Activity or Fragment:

```
RentalStore someRentalShop = RentalStore.getInstance();
ArrayList <Movie> myMovies = someRentalShop.getMovies();
```

## Inter-Fragment communications – interfaces revisited

The main point of Fragments is that they have flexibility and reusability. If you remember, back in Chapter 12, Having a Dialogue with the User, when we were passing Note to and from a Fragment dialog, we added a method to Fragment and then called this method from the instance of the dialog in order to pass in the correct note that is to be shown. And when we added a new note in a dialog, we used getActivity() to get a reference to MainActivity in order to return the new note to be added to ArrayList of notes. Here is the code as a reminder:

```
// Get a reference to MainActivity
MainActivity callingActivity = (MainActivity) getActivity();
// Pass newNote back to MainActivity
callingActivity.createNewNote(newNote);
```

The problem with this is that it assumes that the communication is with an Activity called MainActivity.

Although this works in the Note To Self scenario, it is inflexible because this means that our fragments can only be used in Activities that have the correct methods, or that fragments must have the right methods so that our Activities can be called.

This didn't bother us much in the Note To Self app, but when we will be showing different combinations of fragments on different Activities, then we need a uniform way to communicate, and we need a way to guarantee/force our code that uses our Fragments, to be aware of this communication method.

Interfaces are the solution! Recall that in Chapter 9, Object-Oriented Programming, we saw that an interface is a class in which all the methods have no body and all the methods are abstract by default. So any class that implements the interface must override all its methods using the correct name and parameters.

Take a look at the code in this interface. It has one method,
