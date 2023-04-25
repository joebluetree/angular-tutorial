# Create Modules and components

#### Create a core module
```
ng g m core
```

#### Add core module to app.module
```
  imports: [
    BrowserModule,
    CoreModule
  ],

```



#### Create a Menu Component
```
ng g c core/menu
```

#### export menu component so that it can be accessed in app.module
###### add below code in core.modules.ts
```
  exports: [
    MenuComponent
  ]
```

