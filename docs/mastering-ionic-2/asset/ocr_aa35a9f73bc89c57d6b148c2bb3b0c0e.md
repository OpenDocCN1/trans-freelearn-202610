# ItemSliding and pull-to-refresh

Animations and gestures in modern day applications are a must and they enhance the user experience while using the application features. You will find the `ItemSliding` feature in most iOS applications, and pull-to-refresh is commonly used to fetch the latest data, and as in the Facebook application, you will use this to fetch the latest feeds.

`ItemSliding` is a swipe-able list that can be swiped towards the left or right to reveal action buttons. We will be using a swipe-able list on our wishlist page, where we have products action to delete from the list or add to cart:

```
// src/pages/wishlist/wishlist.ts
<ion-content>
  <ion-list>
    <ion-list-header>
      4 Items in your Wishlist (Swipe for options).
    </ion-list-header>

    <ion-item-sliding *ngFor="let product of products">
      <ion-item>
        <ion-avatar item-left>
          <img [src]="product.image">
        </ion-avatar>
        <h2>{{product.name}}</h2>
        <h3>{{product.price | currency}}</h3>
        <p>{{product.rating}} stars and 420 reviews</p>
      </ion-item>
      <ion-item-options side="right">
        <button danger>
          <ion-icon name="trash"></ion-icon>
          Remove
        </button>
        <button secondary>
          <ion-icon name="cart"></ion-icon>
          Add Cart
        </button>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
</ion-content>
```

Let's look at some of the properties available on `itemSliding`:

+   - Swipe direction can be right or left; both reveal the action buttons. We can use `<ion-item-options side="right">` for right swipe options
+   - IonDrag and `ionSwap` events can be used to listen or call some function during swiping of the list item
+   - Icon placement can be on the top or towards the left of the button text, such as `<ion-item-options icon-left>`
+   - The `ItemSliding` class can also be imported to access the `close()` method