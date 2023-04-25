## Ngrx  Login Screen

#### Actions
```
import { createAction, props } from '@ngrx/store';

export const auth_login = createAction(
  '[Auth] Login',
  props<{ code: string, password: string }>()
);

export const auth_login_success = createAction(
  '[Auth] Login Success',
  props<{ user: iUser }>()
);

export const auth_login_failure = createAction(
  '[Auth] Login Failure',
  props<{ error: string }>()
);

export const auth_logout = createAction('[Auth] Logout')

```

#### Reducer
```
import { createFeatureSelector, createReducer, createSelector, on, props } from '@ngrx/store';

export interface AuthState {
  user?: iUser,
  error?: string
}

const initialState: AuthState = {
  user: undefined,
  error: ''
}

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

```

#### Selectors
```
import { createSelector } from '@ngrx/store';
import { CoreFeatureSelector } from '../index';

export const AuthStateSelector = createSelector(
  CoreFeatureSelector,
  (coreSelectore) => coreSelectore.auth
);

export const selectIsLogin = createSelector(
  AuthStateSelector,
  (state) => state.user ? true : false
);

export const selectIsLogout = createSelector(
  selectIsLogin,
  (isLoggedIn) => !isLoggedIn
);

export const selectUserName = createSelector(
  AuthStateSelector,
  (state) => state.user?.user_name
);

export const selectLoginError = createSelector(
  AuthStateSelector,
  (state) => state.error
);

```

#### Effects
```
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { Store } from '@ngrx/store';
import { catchError, of, tap, switchMap } from 'rxjs';
import { Router } from '@angular/router';

@Injectable()
export class AuthEffects {

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
  }, { dispatch: false });

  constructor(
    private actions$: Actions,
    private loginService: LoginService,
    private router: Router,
    private store: Store
  ) { }

}

```