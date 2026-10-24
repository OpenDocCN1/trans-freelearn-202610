```
antiHeroId) }
}),
on(addAntiHeroState, (state, {antiHero}) => {
    return {...state, antiHeroes: [...state.antiHeroes,
        antiHero]}
}),
on(modifyAntiHeroState, (state, {antiHero}) => {
    return {...state, antiHeroes: state.antiHeroes.map(data
        => data.id === antiHero.id ? antiHero : data)}
}),
);
```

In the preceding code example, we can see that we have added a new reducer for our update feature. This accepts the modified anti-hero data coming from the addAntiHeroState action and returns the new state with the modified antiHeroes value, where we replace the anti-hero with the given ID with the new object using the map() operator.

If the reducer successfully modifies the value of the antiHeroes state, any selectors subscribed to the changes of this state will emit the new value in the component.

# Dispatching the action

The last step we need to do is to dispatch the action to our component. To implement this step, we will do the same steps as we did for the add feature. We will still use the anti-hero/components/anti-hero-form.component.ts file for updating the data.

The only difference here is that we will bind the selected anti-hero value in our form; the anti-hero form component should accept an anti-hero object and should patch the value in the form group.

Let’s have a look at the following code example:

```
export class AntiHeroFormComponent implements OnInit {
    @Input() actionButtonLabel: string = 'Create';
    @Input() selectedAntiHero: AntiHero | null = null;
    @Output() action = new EventEmitter();
    form: FormGroup;
    constructor(private fb: FormBuilder) {
        this.form = this.fb.group({
            id: [''],
            firstName: [''],
```