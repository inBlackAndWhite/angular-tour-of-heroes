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
