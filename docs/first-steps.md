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
```js
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
```js
// src/app/hero.ts

export class Hero {
  id: number;
  name: string;
}
```

In `heroes.component`, let's import the `Hero` class and update the hero property:
```js
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
``
