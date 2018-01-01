# AngularJS

Tutorial: https://angular.io/tutorial/toh-pt0

## Get Started

Create Appication:
```sh
🌹 ng new angular-tour-of-heroes
```

Serve Appication:
```sh
🌹 ng serve --open
```

### Components

Create a component:
```sh
🌹 ng generate component heroes
```

Files generated:
```
  app/heroes/heroes.component.css
  src/app/heroes/heroes.component.html
  src/app/heroes/heroes.component.spec.ts
  src/app/heroes/heroes.component.ts
```

Set properties:
```ts
// app/heroes/heroes.component.ts

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero = 'Windstorm'; // <<<<<<<<<<<< new property

  constructor() { }

  ngOnInit() {
  }

}
```

Display the property in the component template:
```html
<!-- app/heroes/heroes.component.html -->

<h1>{{hero}}</h1>
```

Display the component in the application template
```html
<!-- app/app.component.html -->

<h1>{{title}}</h1>
<app-heroes></app-heroes>
```

#### Create a Class

In order to extend our heroes abilities, let's extract that logic to a separate class:
```ts
// src/app/hero.ts

export class Hero {
  id: number;
  name: string;
}
```

In `heroes.component`, let's import the `Hero` class and update the hero property:
```ts
// app/heroes/heroes.component.ts

import { Hero } from '../hero';

hero: Hero = {
  id: 1,
  name: 'Windstorm'
};
```

Update the application template accordingly to the new property structure:
```html
<!-- app/heroes/heroes.component.html -->

<h2>{{hero.name}} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```

#### Filters

Modify the output with filters:

* Uppercase

hero.name binding:
```html
<!-- app/heroes/heroes.component.html -->
<h2>{{ hero.name | uppercase }} Details</h2>
```

#### Two-way binding

[(ngModel)] is Angular's two-way data binding syntax.

Here it binds the hero.name property to the HTML textbox so that data can flow in both directions: from the hero.name property to the textbox, and from the textbox back to the hero.name.

```html
<!-- app/heroes/heroes.component.html -->

<div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
</div>
```

Since `ngModel` belongs to the FormsModule, we need to import it in order to use it. Import the module and add it to the `@NgModule` external modules array:
```ts
// app.module.ts

import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

#### Repeaters

Let's seed our app some heroes:

```ts
// app/mock-heroes.ts
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

Import the data into the component and create a property:
```ts
// app/heroes/heroes.component.ts
import { HEROES } from '../mock-heroes';

heroes = HEROES;
```

Loop over the object in the template to display its contents:
```html
<!-- app/heroes/heroes.component.html -->

<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```

> The \*ngFor is Angular's repeater directive. It repeats the host element for each element in a list.

Some styles:
```css
.selected {
  background-color: #CFD8DC !important;
  color: white;
}
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes li.selected:hover {
  background-color: #BBD8DC !important;
  color: white;
}
.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.heroes .text {
  position: relative;
  top: -3px;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #607D8B;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
```

#### Event binding

Angular's event binding syntax:
```html
<!-- app/heroes/heroes.component.html -->
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```

Add `onSelect()` function to the component:
```ts
// app/heroes/heroes.component.ts

selectedHero: Hero;

onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```

Update the template with `*ngIf` to display the information only if `selectedHero` is set:
```html
<!-- app/heroes/heroes.component.html -->
<div *ngIf="selectedHero">
  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div><span>name: </span>{{selectedHero.name}}</div>

  <div>
    <label>name:
      <input [(ngModel)]="selectedHero.name" placeholder="name">
    </label>
  </div>
</div>
```

#### Class Binding

The class binding makes allows adding and removing a CSS class conditionally.
```html
<!-- app/heroes/heroes.component.html -->

<li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
```

#### Hero Details Component

Since each component should have its own responsibility, let's split the `HeroesComponent` into two.
So displaying the list of heroes will be managed by `HeroesComponent` and displaying and editing a single hero will be left to `HeroDetailComponent`.

```sh
🌹 ng generate component hero-detail
```

Let's move the single hero template elements to `hero-detail` component.
```html
<!-- app/heroes/hero-detail.component.html -->

<div *ngIf="hero">
  <h2>{{hero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div><span>name: </span>{{hero.name}}</div>

  <div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
  </div>
</div>
```

 Import the `Hero` class to the new component. In order to access the `hero` property from `hero.component` it is necessary to add the Input decorator:
 ```ts
 // app/heroes/hero-detail.component.ts

import { Component, OnInit, Input } from '@angular/core';
import { Hero } from '../hero';

export class HeroDetailComponent implements OnInit {
  @Input() hero: Hero;

  constructor() { }

  ngOnInit() {
  }

}
 ```

Let's add the new component to the `heroes.component` template:
```html
<!-- app/heroes/heroes.component.html -->
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

### Services

Since the logic for fetching data does not belong in the component, it will now be delegated to a service, that can be injected and shared among other components.

```sh
🌹 ng generate service hero
```

```ts
// app/hero.service.ts

import { Injectable } from '@angular/core';
import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable()
export class HeroService {

  constructor() { }

  getHeroes(): Hero[] {
    return HEROES;
  }
}
```

Let's add the service to the dependency injection system and provide it through the `appModule`.
One way of doing this is on service generation:

```sh
🌹 ng generate service hero --module=app
```

This would have added the service to the list of `providers` in the `app.module`, that will create a single instance of the service:
```ts
// /app/app.module.ts
import { HeroService } from './hero.service';

providers: [ HeroService ],
```

Update the component to use the service. Delete the `HEROES` import, import the service and add it to the constructor. Create a function `getHeroes()` to set the data and add a call to the function on `ngOnInit()`:

```ts
// app/heroes/heroes.component.ts

import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes: Hero[];
  selectedHero: Hero;

  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }

  getHeroes(): void {
    this.heroes = this.heroService.getHeroes();
  }

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

}
```
 This approach is fine because data is not being fetched from a remote server. It relies on synchronous response. The service must wait for the data to be delivered. If for some reason the response is delayed, then the browser would freeze until data is actually delivered.

 A better method is to design `getHeroes()` service function to fetch the data asynchronously. One way of doing this is returning an `Observable`.

 Import the Observable in the service and update the `getHeroes()` function:
 ```ts
 // app/hero.service.ts
 import { Observable } from 'rxjs/Observable';
 import { of } from 'rxjs/observable/of';

 getHeroes(): Observable<Hero[]> {
  return of(HEROES);
}
```
> of(HEROES) returns an Observable<Hero[]> that emits a single value, the array of mock heroes.

Subscribe in `HeroesComponent`:
```ts
// app/heroes/heroes.component.ts
getHeroes(): void {
  this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes);
}
```

Now, instead of waiting for a list of heroes from the `getHeroes()`, it waits for an `Observable`. The Observable is responsible for emitting the data when it arrives. Delayed or not, this will prevent the browser from freezing. When the heroes array is delivered from server and emitted by the Observer the the `subscribe()` function passes it as a callback, setting the property.

#### Messages

```sh
🌹 ng generate component messages
```

```html
<!-- app/app.component.html -->
<app-messages></app-messages>
```

```sh
🌹 ng generate service message --module=app
```

```ts
// app/message.service.ts

import { Injectable } from '@angular/core';

@Injectable()
export class MessageService {
  messages: string[] = [];

  add(message: string) {
    this.messages.push(message);
  }

  clear() {
    this.messages = [];
  }
}
```

```ts
// app/hero.service.ts
import { MessageService } from './message.service';

constructor(private messageService: MessageService) { }

getHeroes(): Observable<Hero[]> {
  // Todo: send the message _after_ fetching the heroes
  this.messageService.add('HeroService: fetched heroes');
  return of(HEROES);
}
```

```ts
// app/messages/messages.component.ts
import { MessageService } from '../message.service';

constructor(public messageService: MessageService) {}
```
> Angular only binds to public component properties.

```html
<!-- app/messages/messages.component.html -->

<div *ngIf="messageService.messages.length">

  <h2>Messages</h2>
  <button class="clear"
          (click)="messageService.clear()">clear</button>
  <div *ngFor='let message of messageService.messages'> {{message}} </div>

</div>
```

### Routing

```sh
🌹 ng generate module app-routing --flat --module=app
```

Let's update the router to export the `RouterModule`, so components can access it. Add a constant routes that map `path` to the components. Initialize the router with `imports` and configure it to root level:
```ts
// app/app-routing.module.ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

Update the app template so it displays the components accordingly to the url and a matching nav link:
```html
<!-- app/app.component.html -->
<h1>{{title}}</h1>

<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>

<router-outlet></router-outlet>
<app-messages></app-messages>
```

### Dashboard

```sh
🌹 ng generate component dashboard
```

Fetch the heroes from the service, loop over them to display a name list:
```html
<h3>Top Heroes</h3>

<div class="grid grid-pad">
  <a *ngFor="let hero of heroes" class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>
```

```ts
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```

```css
/* DashboardComponent's private CSS styles */
[class*='col-'] {
  float: left;
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
a {
  text-decoration: none;
}
*, *:after, *:before {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
h4 {
  position: relative;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
  padding: 20px;
  text-align: center;
  color: #eee;
  max-height: 120px;
  min-width: 120px;
  background-color: #607D8B;
  border-radius: 2px;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
  .module {
    font-size: 10px;
    max-height: 75px; }
}
@media (max-width: 1024px) {
  .grid {
    margin: 0;
  }
  .module {
    min-width: 60px;
  }
}
```

Create the route to the `dashboard` and also a default route, that will redirect the user to the dashboard when he visits root path:
```ts
// app/app-routing.module.ts
import { DashboardComponent } from './dashboard/dashboard.component';

const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'heroes', component: HeroesComponent }
];
```

### Hero Details

Let's extract the hero details component from the hero component and place it on its own route
```ts
// app/app-routing.module.ts
import { HeroDetailComponent } from './hero-detail/hero-detail.component';

const routes: Routes = [
  { path: 'detail/:id', component: HeroDetailComponent },
];
```

Update the dashboard and heroes components to display a link to the details:
```html
<a *ngFor="let hero of heroes" class="col-1-4"
    routerLink="/detail/{{hero.id}}">
```

Clean heroes component
```ts
// app/heroes/heroes.component.ts

export class HeroesComponent implements OnInit {
  heroes: Hero[];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}
```

Fetch hero info from id in url in hero-detail component:
```ts
import { Component, OnInit, Input } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { Hero } from '../hero';
import { HeroService }  from '../hero.service';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css']
})
export class HeroDetailComponent implements OnInit {
  @Input() hero: Hero;

  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private location: Location
  ) {}

  ngOnInit() {
    this.getHero();
  }

  getHero(): void {
    const id = +this.route.snapshot.paramMap.get('id');
    this.heroService.getHero(id)
      .subscribe(hero => {
        this.hero = hero
      });
  }
}
```
> The route.snapshot is a static image of the route information shortly after the component was created.

```ts
// app/hero.service.ts
getHero(id: number): Observable<Hero> {
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(HEROES.find(hero => hero.id === id));
}
```

Go back button
```html
<button (click)="goBack()">go back</button>
```

### Persist Data - HTTP - CRUD

Enable HTTP services:
```ts
// app/app.module.ts
import { HttpClientModule } from '@angular/common/http';

imports: [
  BrowserModule,
  FormsModule,
  AppRoutingModule,
  HttpClientModule
]
```

Mimic a remote data server:
```sh
🌹 npm install angular-in-memory-web-api --save
```

```ts
// app/app.module.ts
import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';

imports: [
  BrowserModule,
  FormsModule,
  AppRoutingModule,
  HttpClientModule,
  HttpClientInMemoryWebApiModule.forRoot(
    InMemoryDataService, { dataEncapsulation: false }
  )
]
```

Add data to replace `mock-heroes.ts`:
```ts
// app/in-memory-data.service.ts
import { InMemoryDbService } from 'angular-in-memory-web-api';

export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 11, name: 'Mr. Nice' },
      { id: 12, name: 'Narco' },
      { id: 13, name: 'Bombasto' },
      { id: 14, name: 'Celeritas' },
      { id: 15, name: 'Magneta' },
      { id: 16, name: 'RubberMan' },
      { id: 17, name: 'Dynama' },
      { id: 18, name: 'Dr IQ' },
      { id: 19, name: 'Magma' },
      { id: 20, name: 'Tornado' }
    ];
    return {heroes};
  }
}
```

#### GET

Add the http client to the hero service. Update `getHeroes()` to use it:
```ts
// app/hero.service.ts
import { HttpClient, HttpHeaders } from '@angular/common/http';

private heroesUrl = 'api/heroes';  // URL to web api

constructor(
  private http: HttpClient,
  private messageService: MessageService) { }

getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
}

private log(message: string) {
  this.messageService.add('HeroService: ' + message);
}
```

#### Error handling

```ts
// app/hero.service.ts
import { catchError, map, tap } from 'rxjs/operators';

getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
   .pipe(
     tap(heroes => this.log(`fetched heroes`)),
     catchError(this.handleError('getHeroes', []))
   );
}

getHero(id: number): Observable<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get<Hero>(url).pipe(
    tap(_ => this.log(`fetched hero id=${id}`)),
    catchError(this.handleError<Hero>(`getHero id=${id}`))
  );
}

private handleError<T> (operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {

    // TODO: send the error to remote logging infrastructure
    console.error(error); // log to console instead

    // TODO: better job of transforming error for user consumption
    this.log(`${operation} failed: ${error.message}`);

    // Let the app keep running by returning an empty result.
    return of(result as T);
  };
}
```

#### PUT

```html
<!-- app/hero-detail/hero-detail.component.html  -->
<button (click)="save()">save</button>
```

```ts
// app/hero-detail/hero-detail.component.ts
save(): void {
   this.heroService.updateHero(this.hero)
     .subscribe(() => this.goBack());
 }
```

```ts
// app/hero.service.ts
const httpOptions = {
  headers: new HttpHeaders({ 'Content-Type': 'application/json' })
};

updateHero (hero: Hero): Observable<any> {
  return this.http.put(this.heroesUrl, hero, httpOptions).pipe(
    tap(_ => this.log(`updated hero id=${hero.id}`)),
    catchError(this.handleError<any>('updateHero'))
  );
}
```

#### POST

```html
<!-- app/heroes/heroes.component.html -->
<div>
  <label>Hero name:
    <input #heroName />
  </label>
  <button (click)="add(heroName.value); heroName.value=''">
    add
  </button>
</div>
```

```ts
// app/heroes/heroes.component.ts
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
    this.heroService.addHero({ name } as Hero)
                    .subscribe(hero => this.heroes.push(hero));
}
```

```ts
// app/hero.service.ts
addHero(hero: Hero): Observable<Hero> {
  return this.http.post<Hero>(this.heroesUrl, hero, httpOptions).pipe(
    tap((hero: Hero) => this.log(`added hero w/ id=${hero.id}`)),
    catchError(this.handleError<Hero>('addHero'))
  );
}
```

#### DELETE

```html
<!-- app/heroes/heroes.component.html -->
<button class="delete" title="delete hero" (click)="delete(hero)">x</button>
```

```ts
// app/heroes/heroes.component.ts
delete(hero: Hero): void {
  this.heroes = this.heroes.filter(h => h !== hero);
  this.heroService.deleteHero(hero).subscribe();
}
```

```ts
// app/hero.service.ts
deleteHero (hero: Hero | number): Observable<Hero> {
  const id = typeof hero === 'number' ? hero : hero.id;
  const url = `${this.heroesUrl}/${id}`;

  return this.http.delete<Hero>(url, httpOptions).pipe(
    tap(_ => this.log(`deleted hero id=${id}`)),
    catchError(this.handleError<Hero>('deleteHero'))
  );
}
```
