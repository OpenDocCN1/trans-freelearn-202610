# CRUD operations

To read or write data from the database, we need an instance of database reference. Although we are using AngularFire so these functions are wrapped and we use the function provided by AngularFire library. Sometimes, we have to use low-level methods also like we will be doing later in the project. Let's now first start querying the foodItems to show that on our HomePage:

```
// src/pages/home/home.ts
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { AngularFire, FirebaseListObservable } from 'angularfire2';
import { CityProvider } from '../../providers/city/city'

@IonicPage()
@Component({
  selector: 'page-home',
  templateUrl: 'home.html',
})
export class HomePage {

  foodItems: FirebaseListObservable<any[]>;
  item;
  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public cityService: CityProvider,
    public af:AngularFire) {
```

As we will be querying data by city, we are using cityService, which returns our current location's city according to our geolocation. After this we will only get the data that is from our city:

```
this.cityService.getCityName().then(result => {
    let cityName = result[0]['address_components'][3].long_name;
    this.foodItems = af.database.list('/foodItems', {
      query: {
        orderByChild: 'city',
        equalTo: cityName
      }
    });
  })
}

ratingPage(data:any) {
  console.log(data);
  // In FoodRatingPage we will be showing
  // all the reviews and rating for each item
  this.navCtrl.push("FoodRatingPage", data);
}
}
```

Next, if the user clicks on any of the food item it will show all the reviews and rating associated with that food item.

As mentioned earlier, we are using a city provider which first uses the Ionic Native geolocation plugin and then passes the latitude and longitude to the Google API, which returns use entire detail of that place from street name, the city, state and country. We then parse city name from this and use it in our application:

```
// src/providers/city/city.ts
import { Injectable } from '@angular/core';
```