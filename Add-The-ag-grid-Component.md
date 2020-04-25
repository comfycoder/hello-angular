# Add ag-grid For Angular

From the terminal window, run the following Yarn commands:

```
yarn add @ag-grid-community/all-modules
yarn add @ag-grid-community/core
yarn add @ag-grid-community/angular
```

You should see output similar to the following:

![image.png](/.attachments/image-8a595afa-b842-4065-a7ad-fe207d99a11f.png)

You should see these added to the **package.json** file, as shown below:

![image.png](/.attachments/image-050deac9-d8e4-456d-8a76-47782b2d96b1.png)

Add the **ag-grid** stylesheets to the **styles.scss** file:

![image.png](/.attachments/image-2dde374d-a190-4f47-a84c-0d15d5f820d5.png)

``` scss
@import "~@ag-grid-community/all-modules/dist/styles/ag-grid.css";
@import "~@ag-grid-community/all-modules/dist/styles/ag-theme-balham.css";
```

Add the **AgGridModule** to the **components.module.ts** file.

![image.png](/.attachments/image-af07431e-afe9-43fd-8c84-9c7c9c8d226b.png)

``` typescript
import { NgModule, ModuleWithProviders, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AgGridModule } from '@ag-grid-community/angular';

import { PageHeaderComponent } from './page-header/page-header.component';
import { PageFooterComponent } from './page-footer/page-footer.component';

@NgModule({
  declarations: [
    PageHeaderComponent,
    PageFooterComponent
  ],
  imports: [
    CommonModule,
    RouterModule,
    FormsModule,
    ReactiveFormsModule,
    AgGridModule.withComponents([])
  ],
  exports: [
    CommonModule,
    RouterModule,
    FormsModule,
    ReactiveFormsModule,
    AgGridModule,
    PageHeaderComponent,
    PageFooterComponent
  ]
})
export class ComponentsModule {
  // The forRoot() pattern ensures services are singletons
  static forRoot(): ModuleWithProviders<ComponentsModule> {
    return {
      ngModule: ComponentsModule,
      providers: [

      ]
    };
  }
}
```

Add the **ComponentsModule** to the **dashboard.module.ts** file.

![image.png](/.attachments/image-794b1c38-4545-48f0-9b58-ee833cfb692e.png)

Replace the contents of the **dashboard.component.html** file with the following:

``` html
<h1>Dashboard</h1>

<div class="row">
  <div class="col">
    <div class="card">
      <ag-grid-angular
        id="myGrid"
        class="ag-theme-balham"
        [ngStyle]="{'height.px': gridHeight}"
        [modules]="modules"
        [columnDefs]="columnDefs"
        [rowData]="rowData"
        [frameworkComponents]="frameworkComponents"
        [pagination]="true">

      </ag-grid-angular>
    </div>
  </div>
</div>
```

Replace the contents of the **dashboard.component.ts** file with the following:

``` typescript
import { Component, OnInit } from '@angular/core';
import { AgGridColumn } from '@ag-grid-community/angular';
import {
  Module,
  GridApi,
  ColumnApi
} from '@ag-grid-community/core';
import { AllCommunityModules } from '@ag-grid-community/all-modules';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.scss']
})
export class DashboardComponent implements OnInit {

  public gridHeight = 350;
  public context: any;
  public modules: Module[] = AllCommunityModules;
  public columnDefs: any[];
  public defaultColDef: any = [];
  public columnTypes: any[] = [];
  public rowData: any[] = [];
  public frameworkComponents: {};

  constructor() { }

  ngOnInit(): void {
    this.columnTypes = this.getColumnTypes();
    this.defaultColDef = this.getDefaultColumnDef();
    this.columnDefs = this.getColumnDefs();
    this.rowData = this.getRowData();
    this.frameworkComponents = {};
  }

  getColumnTypes(): any {
    const columnTypes = {
      nonEditableColumn: { editable: false },
      dateColumn: {
        filter: 'agDateColumnFilter',
        filterParams: { comparator: (date1: any, date2: any) => this.dateComparator(date1, date2) },
        suppressMenu: true
      }
    };
    return columnTypes;
  }

  getDefaultColumnDef() {
    const defaultColumnDefs = {
      sortable: true,
      resizable: true,
      // set every column width
      width: 100,
      // make every column editable
      editable: false
    };
    return defaultColumnDefs;
  }

  dateComparator(date1, date2) {
    console.log(`dateComparator: ${date1}, ${date2}`);
    const date1Number = this.monthToComparableNumber(date1);
    const date2Number = this.monthToComparableNumber(date2);
    if (date1Number === null && date2Number === null) {
      return 0;
    }
    if (date1Number === null) {
      return -1;
    }
    if (date2Number === null) {
      return 1;
    }
    return date1Number - date2Number;
  }

  monthToComparableNumber(date) {
    console.log(`monthToComparableNumber: ${date}`);
    if (date === undefined || date === null || date.length !== 10) {
      return null;
    }
    const yearNumber = date.substring(6, 10);
    const monthNumber = date.substring(3, 5);
    const dayNumber = date.substring(0, 2);
    const result = yearNumber * 10000 + monthNumber * 100 + dayNumber;
    return result;
  }

  getColumnDefs(): any[] {
    return [
      { field: 'make' },
      { field: 'model' },
      { field: 'price' }
    ];
  }

  getRowData() {
    return [
      { make: 'Toyota', model: 'Celica', price: 35000 },
      { make: 'Ford', model: 'Mondeo', price: 32000 },
      { make: 'Porsche', model: 'Boxter', price: 72000 }
    ];
  }
}
```

Run the app.

```
npm run start
```

Navigate to the **Dashboard** view.

The Page should appear something like the following:

![image.png](/.attachments/image-7b172109-17ec-442b-83e6-e682ce32be63.png)








