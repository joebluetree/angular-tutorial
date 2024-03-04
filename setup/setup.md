# Setup Anguar App

#### Create new App
```
ng new myApp
```
#### do not install Angular Router 


#### install bootstrap, font-awesome, popper.js
```
npm i bootstrap@latest
npm i font-awesome
npm i popper.js
```
#### Add .css and .js files to angular.json
```
"styles": [
	"src/styles.css",
	"./node_modules/bootstrap/dist/css/bootstrap.min.css",
	"./node_modules/font-awesome/css/font-awesome.css"
],
"scripts": [
	"./node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
]
```
#### Test bootstrap, font-awesome, popper.js
```
<div class="alert alert-primary" role="alert">
  A simple primary alert—check it out!
</div>

<hr>

<div>
  <i class="fa fa-spinner fa-spin fa-3x fa-fw"></i>
  <span class="sr-only">Loading...</span>
</div>

<hr>

<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
    Dropdown button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">Action</a></li>
    <li><a class="dropdown-item" href="#">Another action</a></li>
    <li><a class="dropdown-item" href="#">Something else here</a></li>
  </ul>
</div>

```


