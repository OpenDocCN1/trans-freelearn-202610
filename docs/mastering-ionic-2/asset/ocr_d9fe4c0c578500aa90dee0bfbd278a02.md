# Ionic DB

Ionic DB is the most recent addition in Ionic Cloud services. IonicDB is used to securely store your application data remotely. It integrates well with Auth so users can securely store their data and retrieve it in real time. We will be fetching data in our application from IonicDB and for that we all set to integrate it directly as we have already installed and configured cloud client. Some important features of IonicDB are:

+   - JSON style document storage
+   - Real-time data updation
+   - Authentication and permission manages the read/write privileges.

We will start with creating a database for our application in the dashboard and adding specific collections to it. Collections are a group of documents that are identified by a unique key stored in the id field. You can also think collections similar to what we have tables in relational database. We will be creating some initial collections categories, parent_categories, popular_products, and products, which we will be using inside our application in different views. You will also find user collection, which is automatically created as we have integrated Auth in our application:

![](bbox=[0.153, 0.433, 0.847, 0.688])

We will be using Ionic DB inside multiple views in our application, so for this we have created a DataService provider, which will be directly communicating with Ionic DB and returning data to the views from where the service will be called. This will help us save a lot of code duplicity. Before we get started let's first check out the process of how the Ionic DB works and fetches the data:

```
// src/providers/data-service.ts

import { Injectable } from '@angular/core';
import { Storage } from '@ionic/storage';
import { Database } from '@ionic/cloud-angular';
import { Observable } from 'rxjs/Observable';

import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch'

@Injectable()
export class DataService {

  cartItems: any[];
```