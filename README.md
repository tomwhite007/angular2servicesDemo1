# angular2servicesDemo1
Demo of sharing values between components

```
ng new service-test --routing

cd service-test

ng generate component page1

ng generate component page2

ng generate service my-globals
```


##in app.module.ts...
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

##in app-routing.module.ts...
```javascript
import { Page1Component } from './page1/page1.component';
import { Page2Component } from './page2/page2.component';

const routes: Routes = [  
  { path: 'page1', component: Page1Component },
  { path: 'page2', component: Page2Component }  
];


##in my-globals.service.ts...

export class MyGlobalsService {

  MySharedValue: string;

  constructor() {
    this.MySharedValue = 'start value!';
   }
```

##in app.component.ts...
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

##in app.component.html...
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

##in page1.component.html...
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

##in page1.component.ts...
```javascript
import { MyGlobalsService } from '../my-globals.service' // add import

@Component({
  selector: 'app-page1',
  templateUrl: './page1.component.html',
  styleUrls: ['./page1.component.css']
})
export class Page1Component implements OnInit {

  mySharedValue: string;

  constructor( private globals: MyGlobalsService) { }

  ngOnInit() {
    this.mySharedValue = this.globals.MySharedValue;
  }

  updateService() {
    this.globals.MySharedValue = this.mySharedValue;
  }

}
```

##in page2.component.html...
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

##in page2.component.ts...
```javascript
import { MyGlobalsService } from '../my-globals.service' // add import

@Component({
  selector: 'app-page2',
  templateUrl: './page2.component.html',
  styleUrls: ['./page2.component.css']
})
export class Page2Component implements OnInit {

  mySharedValue: string;

  constructor( private globals: MyGlobalsService) { }

  ngOnInit() {
    this.mySharedValue = this.globals.MySharedValue;
  }

  updateService() {
    this.globals.MySharedValue = this.mySharedValue;
  }

}
```