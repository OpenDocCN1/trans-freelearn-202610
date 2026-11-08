```
{
    provide: AuthService, useValue: authServiceStub
}
]
```

5. Following that, you must configure the testing module to ignore unknown directives by executing the following code:

```
await TestBed.configureTestingModule({
    // [...] 
    schemas: [NO_ERRORS_SCHEMA]
})
```

6. Override the header component and set changeDetection to the default strategy rather than the OnPush strategy, which is incompatible with testing. Here's how to do this:

```
await TestBed.configureTestingModule({
    // [...] 
}).overrideComponent(HeaderComponent, {
    set: {
        changeDetection: ChangeDetectionStrategy.Default
    }
})
.compileComponents();
```

7. Inject the header component's required services just below the compileComponents() method, as follows:

```
authService = TestBed.inject(AuthService);
router = TestBed.inject(Router);
matSnackBar = TestBed.inject(MatSnackBar);
matDialog = TestBed.inject(MatDialog);
```

After we've configured the tests, let's write test cases for the header component, as follows:

8. Write a first test case, as follows:

```
it('should display the app title', () => {
    const h1Element: HTMLElement = fixture.nativeElement
        .querySelector('h1');
```