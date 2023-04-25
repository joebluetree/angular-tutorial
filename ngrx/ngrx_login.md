## Ngrx  Login Steps

####  Steps
```


Create Below Actions
1. Login (code,password)
2. Login Success 
3. Login Failure
4. Logout Action

Create Reducer
1. Create an Interface for Authentication State
2. Create an Initial Authentication State Constant
3. create Reducer
	1. Login Success
	2. Login Failure
	3. Logout
	export const authReducer = createReducer<AuthState>(
	  initialState,
	  on(auth_login_success, (state, payload) => {
	    console.log('login success ', payload.user);
	    return {
		user: payload.user,
      	error: ''
	    }
	  }),
	  on(auth_login_failure, (state, payload) => {
	    console.log('login Failure ', payload);
	    return {
	      user: undefined,
	      error: payload.error
	    }
	  }),
	  on(auth_logout, (state) => {
	    return initialState;
	  }),
	)


4. Create Root Store
	1. Create an interface for Root State
		export interface CoreState {
		  auth: AuthState
		}
	2. Create root reducers for all reducers
		export const coreReducers: ActionReducerMap<CoreState> = {
		  auth: authReducer
		}
	3. create a constant for root_feature_name
		export const CORE_FEATURE_NAME = "core";
	4. Create feature Selector
		export const CoreFeatureSelector = createFeatureSelector<CoreState>(CORE_FEATURE_NAME);
	
5. Create Selector
	1. Create RootStateSelector using feature Selector
		export const AuthStateSelector = createSelector(
		  CoreFeatureSelector,
		  (coreSelectore) => coreSelectore.auth
		);
	2. Create Selector to check IsLogin
	3. create selector for IsLogout
	4. create selector to get user name
	3. create selector to get error
6. Create Effects
	1. Create Login Effect to call login service
	  login$ = createEffect(() => {
	    return this.actions$.pipe(
	      ofType(auth_login),
	      switchMap(user => this.loginService.login(user).pipe(
	        tap((user: any) => {
	          if (user == undefined) {
	            this.store.dispatch(auth_login_failure({ error: 'Login Error' }));
	          }
	          else {
	            const _user: iUser = {
	              user_id: user.user_id,
	              user_code: user.user_code,
	              user_name: user.user_name,
	              user_email: user.user_email,
		        user_password: ''
	            }
	            localStorage.setItem("token", JSON.stringify(user));
	            this.store.dispatch(auth_login_success({ user: _user }));
	            this.router.navigate(['/home']);
	          }
	        }),
	        catchError((err) => {
	          this.store.dispatch(auth_login_failure({ error: err.error }));
	          return of(err.error);
	        })
	      ))
	    )
	  },{ dispatch: false });

	  constructor(
	    private actions$: Actions,
	    private loginService: LoginService,
	    private router: Router,
	    private store: Store
	  ) { }


```