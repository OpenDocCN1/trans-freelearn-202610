Figure 10.2 – Folder structure and dependencies installed after scaffolding

To run our project, we can open the VS Code terminal with the Ctrl + ` shortcut and execute the ng serve command. We can also use the defined scripts in our package.json file. In this case, we can perform npm run start to run our application. We will see in the terminal whether Angular is running successfully on our local server in the following screenshot:

```
{
  "name": "superheroes",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "watch": "ng build --watch --configuration development",
    "test": "ng test"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "~13.3.0",
    "@angular/common": "~13.3.0",
    "@angular/compiler": "~13.3.0",
    "@angular/core": "~13.3.0",
    "@angular/forms": "~13.3.0",
    "@angular/platform-browser": "~13.3.0",
    "@angular/platform-browser-dynamic": "~13.3.0",
    "@angular/router": "~13.3.0",
    "rxjs": "~7.5.0",
    "tslib": "^2.3.0",
    "zone.js": "~0.11.4"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "~13.3.4",
    "@angular/cli": "~13.3.4",
    "@angular/compiler-cli": "~13.3.0",
    "@types/jasmine": "~3.10.0",
    "@types/node": "~12.11.1",
    "jasmine-core": "~4.0.0",
    "karma": "~6.3.0",
    "karma-chrome-launcher": "~3.1.0",
    "karma-coverage": "~2.1.0",
    "karma-jasmine": "~4.0.0",
    "karma-jasmine-html-reporter": "~1.7.0",
    "typescript": "~4.6.2"
  }
}
```