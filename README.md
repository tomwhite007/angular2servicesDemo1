# angular2servicesDemo1
Demo of sharing values between components

This was created using Angular CLI at v1.0.0-rc.2

In this Part 1 we can see how easy it is to set up global variables in a service.  

Note, the Typescript code extracts below only need to be changed where I've added comments on the end of the line. 

I've used a button click to update the variable in the service because it's easier to demonstrate the pros and cons of using a basic variable in a service to share state. The problem with this design is that app.component heading won't change because it hasn't been told to refresh.```
ng new service-test --routing

cd service-test

ng generate component page1

ng generate component page2

ng generate service my-globals
```


in app.module.ts...
```javascript
import { MyGlobalsService } from './my-globals.service' // add import

@NgModule({
  declarations: [
    AppComponent,
    Page1Component,
    Page2Component
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule
  ],
  providers: [MyGlobalsService], // add provider
  bootstrap: [AppComponent]
})
```

in app-routing.module.ts...
```javascript
import { Page1Component } from './page1/page1.component'; // add import
import { Page2Component } from './page2/page2.component'; // add import

const routes: Routes = [  
  { path: 'page1', component: Page1Component }, // replace routes
  { path: 'page2', component: Page2Component }   // replace routes
];
```

in my-globals.service.ts...
```javascript
@Injectable()
export class MyGlobalsService {

  MySharedValue: string;  // add

  constructor() {
    this.MySharedValue = 'start value!';  // add
  }

}
```

in app.component.ts...
```javascript
import { MyGlobalsService } from './my-globals.service' // add import

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
  mySharedValue: string; // add

  constructor( private globals: MyGlobalsService) { } // add

  ngOnInit() {
    this.mySharedValue = this.globals.MySharedValue;  // add
  }

}
```

in app.component.html...
```html
<h1>
  {{title}} {{mySharedValue}}
</h1>

<p>
  <a routerLink="/page1" routerLinkActive="active" class="nav-but">Page 1</a>&nbsp;
  <a routerLink="/page2" routerLinkActive="active" class="nav-but">Page 2</a>&nbsp;
</p>

<router-outlet></router-outlet>
```

in page1.component.html...
```html
<p>
  page1 works!
</p>
<p>
  <input [(ngModel)]="mySharedValue" type="text" />
</p>
<p>
  <button type="button" (click)="updateService()">Update Globals Service</button>  
</p>
```

in page1.component.ts...
```javascript
import { MyGlobalsService } from '../my-globals.service' // add import

@Component({
  selector: 'app-page1',
  templateUrl: './page1.component.html',
  styleUrls: ['./page1.component.css']
})
export class Page1Component implements OnInit {

  mySharedValue: string;  // add

  constructor(private globals: MyGlobalsService) { }  // insert

  ngOnInit() {
    this.mySharedValue = this.globals.MySharedValue;  // add
  }
  
  updateService() {
    this.globals.MySharedValue = this.mySharedValue;  // add
  }

}
```

in page2.component.html...
```html
<p>
  page2 works!
</p>
<p>
  <input [(ngModel)]="mySharedValue" type="text" />
</p>
<p>
  <button type="button" (click)="updateService()">Update Globals Service</button>  
</p>
```

in page2.component.ts...
```javascript
import { MyGlobalsService } from '../my-globals.service' // add import

@Component({
  selector: 'app-page2',
  templateUrl: './page2.component.html',
  styleUrls: ['./page2.component.css']
})
export class Page2Component implements OnInit {
  mySharedValue: string;  // add

  constructor(private globals: MyGlobalsService) { }  // insert

  ngOnInit() {
    this.mySharedValue = this.globals.MySharedValue;  // add
  }
  
  updateService() {
    this.globals.MySharedValue = this.mySharedValue;  // add
  }

}
```
