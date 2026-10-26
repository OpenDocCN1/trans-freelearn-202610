```
provide: TranslateLoader,
useFactory: createTranslateLoader,
deps: [Http]
})
],
providers: [
{ provide: ErrorHandler, useClass: IonicErrorHandler},
{ provide: Storage, useFactory: provideStorage },
{ provide: APP_BASE_HREF, useValue: '/'},
UserService,
DataService
]

// Deeplink Configuration
export const deepLinkConfig: DeepLinkConfig = {
links: [
{ component: HomePage, name: 'Home Page', segment: '' },
{ component: CategoriesPage, name: 'Categories Page',
segment: 'categories' },
{ component: ProductsPage, name: 'Categories Product
Page', segment: 'categories/:categoryId' },
{ component: ProductDetailPage, name: 'Product Details
Page', segment: 'products/:productId' },
{ component: WishlistPage, name: 'Wishlist Page',
segment: 'wishlist' },
{ component: ShowcartPage, name: 'Showcart Page',
segment: 'cart' },
{ component: SupportPage, name: 'Support Page', segment:
'feedback' },
{ component: SettingsPage, name: 'About Page', segment:
'settings' },
{ component: AboutPage, name: 'About Page', segment:
'about' },
{ component: LoginPage, name: 'Login Page', segment:
'login' },
{ component: SignupPage, name: 'Signup Page', segment:
'signup' },
{ component: AccountPage, name: 'Account Page', segment:
'account' }
]
};
```

Now, similar to how you open a specific link with a unique URL, you can go to a specific view with a custom URL scheme. So now our application can be accessed at vplanet://wishlist, which will now open the application's wishlistPage directly.