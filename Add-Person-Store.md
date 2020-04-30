# Summary

Now we'll add our first in-memory data store.

In the src\app\stores folder, create a new file called:

```
person.store.ts
```

Copy the following code into the **person.store.ts** file:

``` typescript
import { Injectable } from '@angular/core';
import { Store } from './abstract-base.store';
import { map } from 'rxjs/operators';
import { Person } from '../models/person.model';

class PersonState {
    person: Person;
}

const INITIAL_STATE: PersonState = {
    person: undefined
};

@Injectable({
    providedIn: 'root'
})
export class PersonStore extends Store<PersonState> {

    constructor(
    ) {
        super(INITIAL_STATE);
    }

    selectPerson() {
        return this.state$
            .pipe(map((state: PersonState) => {
                console.log('selectPerson: ', state?.person);
                return state?.person;
            }));
    }

    clearState() {
        this.setNextState(INITIAL_STATE);
    }
}
```

## Add PersonStore to the main stores.module.ts file

In VS Code, navigate to the new **stores.module.ts** file in the **src/app/stores** folder.

Edit the module as shown in the following:

``` typescript
import { NgModule, Optional, SkipSelf, ModuleWithProviders } from '@angular/core';
import { CommonModule } from '@angular/common';
import { PersonStore } from './person.store';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class StoresModule {

  constructor(@Optional() @SkipSelf() parentModule: StoresModule) {
    if (parentModule) {
      throw new Error(
        'StoresModule is already loaded. Import it in the AppModule only');
    }
  }

  static forRoot(): ModuleWithProviders<StoresModule> {
    return {
      ngModule: StoresModule,
      providers: [
        PersonStore
      ]
    };
  }
}
```

In the terminal window, verify there are no issues reported:

![image.png](/.attachments/image-a5381f82-8d39-4693-bca2-57286c742a8a.png)

Run the app.

```
npm run start
```

You have now added the Person data store.