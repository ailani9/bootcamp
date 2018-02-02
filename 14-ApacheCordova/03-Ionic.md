# [Ionic](https://ionicframework.com/)

* [Sign up](https://dashboard.ionicjs.com/signup) for Ionic Pro.
* Choose the free kickstarter plan.
* Go to the [Geetting Started](https://ionicframework.com/getting-started) guide.
* Install Ionic and create your first app

```sh
npm install -g ionic
ionic start myApp tabs
```

* Review the file structure and draw comaprisons to Angualar.
* Exit this program and create the CMS app
```
ionic start ionic-cms sidemenu
ionic serve
```

## Create a users page and wire it into navigation.

1. Import the UserProvider (aka Service in Angular)
2. Import the User schema/model
3. Declare users as an Array containing user objects
4. Inject the UserProvider
5. Create a wrapper for the users provider
6. call the getUsers() wrapper

```sh
ionic generate page users
```

[</> code](https://github.com/microtrain/ionic-cms/commit/e596528a24bc6e432fc634808fc378756610a3f0)
Declare UsersPage in app.module.ts

[</>  code](https://github.com/microtrain/ionic-cms/commit/52c30d29da7b72ffa6920aa95ae50900bbad07f5)
Add the UserPage to your side menu.

Create a data provider to connect to the users API.
```sh
ionic generate provider user
```

UserProvider will give us access to a users API. We will create a getUsers() method that will return a list of users; this is the goal of UsersPage. To allow UsersPage to access UserProvider we will need to import and inject it into UsersPage.

[</> code](https://github.com/microtrain/ionic-cms/commit/708980c169ca58a8e6b76a3aa036430ab361c3e7) Import the UserProvider and inject it into UserPage.

At this point your application should crash when attempting to access the UsersPage. This is because the data provider (UserProvider) is a calling HttpClient but HttpClientModule is not getting loaded. To resolve this issue we will load it into app.module.ts.

[</> code](https://github.com/microtrain/ionic-cms/commit/f71c37d378356286e1430d57af46163965bef089) Add HttpClientModule

It's always good to build one piece at a time. We want to make sure the connection between our provider and controller (UsersPage) is working. While we may not yet be ready to implement each provider method it's a good practice to make sure we've accounted for everything we will need to work with the API. In this case we will be dealing with basic CRUD (Create, Read, Update, Delete) logic. This tranlates in to
* get a user ```getUser()```
* get many users ```getUsers()```
* create a user ```createUser()```
* update a user ```updateUser()```
* delete a user ```deleteUser()```

[</> code](https://github.com/microtrain/ionic-cms/commit/7101ff4bb7163d3e6a7781317bae6502208f775a) Stub out the user provider with the soon to be used methods. Start with a having each method execute simple ```console.log()```.

We will want to create a user data object, other paradigms may refer to this as a model or a schema. For the sake of argument we will call it a model. For now we will just define the class. Create a models dirctory and user model *models/user/user.ts*

[</> code](https://github.com/microtrain/ionic-cms/commit/2a47fa9797f86e22e163dc101e926534b343d8bd) Stub a User object.

[</> code](https://github.com/microtrain/ionic-cms/commit/635f989e910a2aa409d78b190855753bd5d3b41e) Import the User object (model) into UserProvider.

Before we begining implementing UserProvider against the API lets create a wrapper in out UserPage and test the connection to the provider. To do this we will create a private method that calls the ```getUsers()``` method in UserProvider. We will call that method from the UserPage constructor. Opening the JS console in the dev tools panel then navigationg to the users page in our app will now display "Get Users". At this point we know we have a good connection and we can focus on implementing the ```getUsers()``` logic.

[</> code](https://github.com/microtrain/ionic-cms/commit/2bc571d53cc0fc4a29fb1ed5d63cbb8e278301c5) Implement a basic wrapper for ```getUsers()```.

Now that we have a basic connection between UserProvider and UserPage we will want to implement an API call against ```UserProvider.getUsers()```. This will require the following steps.
1. Import Observable from rxjs
1. Set the base URL inside of UserProvider
1. Create an Observable of type User and implemnet the get logic
1. Subscribe to the observable and catch the response.

[</> code](https://github.com/microtrain/ionic-cms/commit/64e3eff7bb1726a4ab7b97071be6568481a765e4) Implement the ```getUsers()``` API call.

Opening the JS console in the dev tools panel then navigationg to the users page in our app will now display a JSON object containing a an Array of users.

Make the uses list public so that it will be accessible by the view.

1. Create a public instance varaible to hold the list of users. This will hold an Array of user objects.
2. Set the list of users the users instance variable. This will give view access to the list of users.
3. Log the current state of the users instance.

[</> code](https://github.com/microtrain/ionic-cms/commit/a613fe7b1382ac84e13400aff2548603ad51ad37) Add the users data to a public variable.

At this point the ```console.log()``` will show a deliniated list of users.

Our view now has access to the public users instance. We can display dispay a list of users in the view using [Angular's ngForOf directive](https://angular.io/api/common/NgForOf) to build an [Ionic list](https://ionicframework.com/docs/components/#lists). You can now remove the ```console.log()``` from UsesPage.

[</> code](https://github.com/microtrain/ionic-cms/commit/67a8b26843b56478bf9976b96248d9c072bb1dcb) Implement the users view.

### Add a Loader

Our data comes from a web API. This means any network latency can make a page load feel sluggish or even broken. Using a loader is a good way to ease the pain ofa slow page load. 

1. Import LoadingController
2. Inject the LoadingController into the constructor
3. Create a space in memory to hold a loader (an instance variable)
4. Create a method and display a loader
5. Call the loaded when requesting user data
6. Dismiss the loader after the HTTP request has completed

[</> code](https://github.com/microtrain/ionic-cms/commit/600212a852488d9d6fc3afc86774564ab0004b0e) Add a loader.

[</> bug fix](https://github.com/microtrain/ionic-cms/commit/a407dfd23fb4e10e71ccfad0d963bd3b874dfb53) The previous commit was missing an import.

[</> bug fix](https://github.com/microtrain/ionic-cms/commit/ae46e812eaf1c0a5d5cf7301557fb3d130c0ab7d) Typing issues brought to the surface byt the previous fix.


### Create UserPage and Display a Single User

Implement a page to display a single user.

1. Generate a user page
1. Import the UserProvider (aka Service in Angular)
1. Import the User model
1. Declare a public user variable
1. Create a wrapper for the UserProvider.getUser()
1. Call the getUsers() wrapper
1. Build the view
1. Link the users list to the new view


```sh
ionic generate page user
```

Add UserPage to NgModule.entryComponents

*pages/users/users.ts*
```js
import { Component } from '@angular/core';

import { IonicPage, NavController, NavParams, LoadingController } from 'ionic-angular';

import { UserProvider } from '../../providers/user/user';

import { User } from '../../models/user';
//1. Import UserPage
import { UserPage } from '../user/user';

@IonicPage()
@Component({
  selector: 'page-users',
  templateUrl: 'users.html',
})
export class UsersPage {

  public users: User;

  //2. Pass the Userpage object into view
  public toUser = UserPage;

  private loader: any;

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    private userProvider: UserProvider,
    //3. Inject the LoadingController
    public loadingCtrl: LoadingController
  ) {
    //4. Build and display a loader on instaniation
    this.loader = this.loadingCtrl.create({
      content: 'Loading...',
    });

    this.loader.present();
  }

  public ionViewDidLoad() {
    this.getUsers();
    this.userProvider.getUser();
    this.userProvider.editUser();
    this.userProvider.createUser();
    this.userProvider.deleteUser();
  }


  public getUsers(): void {
    this.userProvider.getUsers().subscribe(
      (response) => {
        this.users = response.users,
        console.log(this.users),
        //5. Dismiss the loader after the Http Request has completed
        this.loader.dismiss()
      }
    );
  }

}
```

*pages/users/users.html*
```html
<ion-content padding>
  <ion-list *ngIf="users">
    <button ion-item *ngFor="let user of users" [navPush]="toUserDetail" [navParams]="user._id">
      {{user.username}}
    </button>
  </ion-list>
</ion-content>
```






