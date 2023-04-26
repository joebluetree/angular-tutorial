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
	1. create interface iModule
		export interface iModulem {
		  module_id: number;
		  module_name: string;
		  module_is_installed: string;
		  module_order: number;
		}
		export interface iModulem_Search {
		  module_name: string;
		  module_is_installed: string;
		}
		export interface iPage {
		  currentPageNo: number;
		  pages: number;
		  pageSize: number;
		  rows: number;
		}
	2. Create interface for state which extends EntityState<iModulem>
		export interface ModuleState extends EntityState<iModulem> {
		  selectid: number;
		  search_record: iModulem_Search;
		  page: iPage;
		  sort_column: string;
		  sort_order: string;
		  error: string;
		};
	3. Create An Adapter - CreateEntityAdapter<iModulem> 	
		const adapter: EntityAdapter<iModulem> = createEntityAdapter<iModulem>({
		  selectId: (m) => m.module_id
		});
	4. Get Initial State from adapter
		export const initialState: ModuleState = adapter.getInitialState({
		  selectid: 0,
		  search_record: <iModulem_Search>{ module_name: '', 						  module_is_installed: 'NA' },
		  page: <iPage>{ currentPageNo: 1, pages: 0, pageSize: 10, rows: 0 },
		  sort_column: 'module_order',
		  sort_order: 'asc',
		  error: ''
		});
	5. Create Reducer CreateReducer<ModuleState>
		export const moduleReducer = createReducer<ModuleState>(
		  initialState,
		  on(module_load_success, (state, action) => {
		    return adapter.setAll(action.records, { ...state, page: action.page, 			error: '' });
		  }),
		  on(module_load_failure, (state, action) => {
		    return adapter.removeAll({ ...state, error: action.erorr })
		  }),
		  on(module_update_selected_rowid, (state, action) => {
		    return { ...state, selectid: action.id };
		  }),
		  on(module_update_search, (state, action) => {
		    return { ...state, search_record: action.search_record }
		  }),
		)
	6. Get Selectors from adapter adapter.getSelectors();
		export const { selectAll, selectEntities, selectIds, selectTotal } = 				adapter.getSelectors();
	7. Set FeatuerName and Create FeatureSelector
		export const ModuleFeatureName = 'moduleState';
		export const moduleFeature = createFeatureSelector<ModuleState>					(ModuleFeatureName);
```

####  Module  Reference
```	
	1.	StoreModule.forFeature(ModuleFeatureName, moduleReducer),
	2.    EffectsModule.forFeature([ModuleEffects])
```

####  Create Selector
```
	1. create  selector from FeatureSelector
		export const moduleSelector = createSelector(
		  moduleFeature,
		  selectAll
		)
		export const moduleState = createSelector(
		  moduleFeature,
		  (state: ModuleState) => state
		)
```

####  Create Effects
```	
@Injectable()
export class ModuleEffects {
  moduleList$ = createEffect(() => {
    return this.actions$.pipe(
      ofType(module_load_records),
      withLatestFrom(
        this.store.select(moduleSearch_Record),
        this.store.select(modulePage)
      ),
      switchMap(([action, search_record, page]) => {
        const data: any = this.service.getList(action.action, search_record, page);
        console.log(data);
        return data;
      }),
      tap((result: any) => {
        console.log('Module List', result);
        return this.store.dispatch(module_load_success({ records: result.records, page: result.page }));
      })
    );
  }, { dispatch: false });

  moduleDelete$ = createEffect(() => {
    return this.actions$.pipe(
      ofType(module_delete),
      switchMap((action: any) => this.service.delete(action.id)),
      tap((result: any) => {
        if (result.status)
          this.store.dispatch(module_delete_complete({ id: result.id }));
        else {
          throw new Error(result.message);
        }
      }),
      catchError(error => {
        const err = !!error.error ? error.error : error;
        this.gs.showScreen([err]);
        throw error;
      })
    );
  }, { dispatch: false });


  constructor(
    private actions$: Actions,
    private service: ModuleService,
    private store: Store<ModuleState>,
    private gs: GlobalService
  ) {
  }
}


```
