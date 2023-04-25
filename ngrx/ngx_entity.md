## Ngrx  Entity

####  Create Actions
```
	1. Load Module
	2. Load Module Success
	3. Load Modules Falure
	4. Update Selected RowID
	5. Update Search
	6. Upsert Row
	7. Delete
	8. Delete Complete
	9. Sort
```
####  Create Reducer
```
	1. Create State ModuleState extends EntityState<iModulem>
	2. Create An Adapter - CreateEntityAdapter<iModulem> 	
	3. Get Initial State from adapter
	4. Create Reducer CreateReducer<ModuleState>
	5. Get Selectors from adapter adapter.getSelectors();
	6. Set FeatuerName
	7. Create FeatureSelector
```


####  Create Selector
```
	1. create  selector from FeatureSelector
```

####  Create Effects
```	
	1. Create Effects
```
