```
```typescript
@Output('user')
userEmitter = new EventEmitter<User>();
  selectUser(user:User) {
        this.userEmitter.emit(user);
  }
}
```
```

In the preceding example code, we have created a separate component for the table that displays the list of users. Since dumb components have no dependencies injected, the component will need to receive the data from the smart components. To achieve this, we have added an @Input binding property to accept the list of users from HomeComponent; on the other hand, we have also added an @Output binding property to bubble actions to the parent or smart component.

Remember that dumb components must not have any logic or actions; in this case, we will pass the event in the parent component with the use of EventEmitter and the parent component will be responsible for the steps needed to be accomplished. In the example code, once a row is clicked, we are passing the user to the userEmitter that HomeComponent will retrieve.

After successfully creating UserListComponent, we can now use this in our HomeComponent, and we will have the following code:

```
```typescript
@Component({
  selector: 'app-home',
  template: `
    <h2>User List</h2>
    <div>
        <users-list users="users"
          (user)="selectUser($event)"/>
    </div>
  `,
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  users: User[] = [];
  constructor(private userService: UserService) {
  }
  ngOnInit() {
      this.userService.getUsers()
```
```