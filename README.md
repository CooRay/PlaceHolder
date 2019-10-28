### Project Instructions

1. Back on your local machine, use the Angular CLI to:
2. Create a new application called `PlaceHolder`
```
ng new Placeholder
```
3. Create a service file called `placeholder` service.
```
ng generate service /services/Placeholder
```
4. Import the necessary modules to make REST calls.
    > *In your app.module.ts, import the 'HttpClientModule' from '@angular/common/http'*
5. Create a todo model usning: `ng generate interface /interfaces/ITodo` and add these properties:
```typescript
export interface ITodo {
    userId: number;
    id: number;
    title: string;
    completed:boolean;
}

```
6. Create a GET service method
7. Inside that GET service method, call `https://jsonplaceholder.typicode.com/todos`.  It should all look like this:
```typescript
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";
import { ITodo } from "../interfaces/itodo";

@Injectable({
  providedIn: "root"
})
export class PlaceholderService {
  constructor(private http: HttpClient) {}

  get(): Observable<ITodo> {
    return this.http.get<ITodo>("https://jsonplaceholder.typicode.com/todos");
  }
}
```
8. In the `app.component.ts` inject the placeholder service
```typescript
import { Component } from '@angular/core';
import { PlaceholderService } from './services/placeholder.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  constructor(placeholderService: PlaceholderService){}
}
```
9. In the `app.component.html` file Create a  table with the following headers:
```html
<table>
  <tr>
    <th>Title</th>
    <th>Completed</th>
  </tr>
</table>
```
10. Populate the table rows with data return to the REST method using `*ngFor'.

### app.component.ts

```typescript
import { Component } from '@angular/core';
import { PlaceholderService } from './services/placeholder.service';
import { ITodo } from './interfaces/itodo';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  todos: ITodo[];
  constructor(placeholderService: PlaceholderService){
    placeholderService.get()
    .subscribe(data=> this.todos = data);
  }
}
```

### app.component.html
```html
<table>
  <tr>
    <th>Title</th>
    <th>Completed</th>
  </tr>
  <tr *ngFor='let todo of todos'>
    <td>{{todo.title}}</td>
    <td>{{todo.completed}}</td>
  </tr>
</table>
```
