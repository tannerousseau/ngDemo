# NgDemo

A demonstration of how angular apps can be structured.

Assumes Node has already been installed, as well as the angular CLI.\
https://cli.angular.io/

## CLI commands to create components

### Create App

To create a new Angular app named "demoApp"\
`ng new demoApp`\
`cd demoApp`

This app can be run using ng serve and opening localhost:4200/ in a browser.

### Generate structure component

Generate a component named "structure". This will be the base of our app where we will display other components. \
`ng generate component structure`

This will create a "structure" folder in the src/app folder, and the files required for the component. The component has also been imported into the app.module.ts file.

Set the structure component as the starting point of our app, by placing its selector in the `app.component.html`

```html
<app-structure></app-structure>
```

### Generate landing page and navigation bar

Generate a landing and navBar components in the structure folder. Shorthands `g` and `c` are used in place of `generate` and `component`.\
`ng g c structure/landing`\
`ng g c structure/navBar`

Add the navbar to the `structure.component.html`. The router outlet is where our components will be displayed when we navigate to them. More on the router below.

```html
<app-nav-bar></app-nav-bar> <router-outlet></router-outlet>
```

### Generate components

Generate a few components to actually show some content.\
`ng g c components/widgetA`\
`ng g c components/widgetB`

### Configure Routing

The "routes" array in `src\app\app-routing.module.ts` maps a url path to a component.

```ts
const routes: Routes = [
  { path: '', component: LandingComponent }, // base route
  { path: 'widgetA', component: WidgetAComponent },
  { path: 'widgetB', component: WidgetBComponent },
];
```

Add buttons in `src\app\structure\nav-bar\nav-bar.component.html` and assign the desired link to the `routerLink` attribute.

```html
<button routerLink="/">Landing</button>
<button routerLink="/widgetA">widgetA</button>
<button routerLink="/widgetB">widgetB</button>
```

### Add a Service
Add a service for sharing data between components
`ng g s data`

Add some simple data to the data service here:
`src\app\data.service.ts`
```ts
  data = [
    'First piece of default Data',
    'Second piece of default data'
    ];
```

### Display service data in a component
Inject the data service into our components constructor. For example, in widgetA:
`src\app\components\widget-a\widget-a.component.ts`
```ts
constructor(private dataService: DataService) {}
ngOnInit(): void {
  this.data = this.dataService.data;
}
```

Now use this data in widgetA's html template:
`src\app\components\widget-a\widget-a.component.html`
```html
<p *ngFor="let element of data">{{ element + '!' }}</p>
```
There is a lot going on in this one line.

 `*ngFor` is an Angular structural directive, meaning it modifies the structure of the html document. In this case, it is a for loop that loops through the `data` array and creates `<p>` tags filled with the arrays elements.

 The `{{ ... }}` syntax is specific to Angular, and allows us to write code in the `<p>` tag. In the case above, for each `element` in the `data` array, which is a strings, add an exclamation point!

### Modify the service data in a component
Inject the data service into widgetB in the same way, but we also add an `inputText` property, and the method `onClick` that calls the data service method 'addData'.\
`src\app\components\widget-b\widget-b.component.ts`

Then add an input tag and button to widgetB's html template.\
`src\app\components\widget-b\widget-b.component.html`
```html
<div>
    <input type="text" [(ngModel)]="inputText" value="inputText" />
    <button (click)="onClick()">Add to data service</button>
</div>

<hr />
<p>{{ inputText }}</p>
```
Again, there is a lot going on here:\
`[(ngModel)]="inputText"` creates a two way connection between the component variable `inputText` and the value displayed in the input field. If we change the text in the input field, it will change the component variable, and the other way around. This is "two way binding" in Angular jargon. To use this special syntax, we need to import the `FormsModule` in `src\app\app.module.ts`.

`value="inputText"` sets the initial value of the input field. This is normal html, not Angular specific.

`(click)="onClick()"` binds the `onClick()` method to the `click` output of the button. Meaning, when we click the button, the `onClick` method will be called. This syntax IS Angular specific.

`<p>{{ inputText }}</p>` is displaying the current value of the component property `inputText`. This demonstrates that the `inputText` is being update when the text input changes.
## Prettier (optional)

Automating code formatting in vscode.

Basically this article:\
https://medium.com/@victormejia/setting-up-prettier-in-an-angular-cli-project-2f50c3b9a537

`npm install prettier -D`

In `.vscode\settings.json` the set automatic formatting on save, and set the config file for prettier. Add any customization to the `.prettierrc` file. These files may need to be created manually.

```json
{
  "editor.formatOnSave": true,
  "prettier.configPath": ".prettierrc"
}
```

To format all files, run the command:\
 `npx prettier --write .`

## Recommended vsCode extensions
- Angular Language Service
- Code Spell Checker
- GitLens