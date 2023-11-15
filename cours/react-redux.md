# React Redux

## But

Utiliser le store redux avec une application React.

## Installation

```bash
pnpm install react-redux
```

## Configuration

### Mise en place du Provider

Il faut englober notre application avec le `Provider` de redux qui contiendra le store

```tsx
// ./src/main.tsx
import { Provider } from 'react-redux';
import store from './store';

// ...
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### Mise en place des hooks pré typer (parce qu'on est en TypeScript)

#### Typage côté store

[Voir la doc](https://redux-toolkit.js.org/tutorials/typescript#define-root-state-and-dispatch-types)

```ts
// ./src/store/index.ts
const store = configureStore({
  //...
});

// Je créer des type qui vont être déduis à partir du store
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

#### Typage des hooks de react redux

Le but de ses hooks est d'avoir des hooks avec le typage du store de pré définis.
Ca évitera de devoir le faire à chaque utilisation du `useSelector`.

[Voir la doc](https://redux-toolkit.js.org/tutorials/typescript#define-typed-hooks)

```tsx
// ./src/hooks/redux.ts
import { useDispatch, useSelector } from 'react-redux';
import type { TypedUseSelectorHook } from 'react-redux';
import type { RootState, AppDispatch } from '../store';

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch: () => AppDispatch = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

## Utilisation dans nos composants

### useSelector / useAppSelector

`useAppSelector` va être un hook qui nous permet de "piocher" les données qui nous intéresse dans notre store.
Il a la particularité de récupérer la donnée ET de s'abonner à ses modifications.
Si la donnée retourner par le `useAppSelector` change, le composant sera re-rendu.

```tsx
function ShowCounter() {
  // Je récupère la donnée que je stock dans une variable `countValue`
  const countValue = useAppSelector((state) => state.counter.count);

  return <div>{countValue}</div>;
}
```

### useDispatch / useAppDispatch

`useAppDispatch` va être un hook qui nous permet de récupérer la fonction `dispatch` du store.
Cette fonction `dispatch` récupérer nous permettre d'émettre nos intentions à notre store.

```tsx
// ./src/components/IncrementCounter/IncrementCounter.tsx
import { increment, incrementBy } from '../../store/reducers/counter';

function IncrementCounter() {
  // Je récupère la fonction `dispatch` que je stock dans une variable `dispatch`
  const dispatch = useAppDispatch();

  // Au click sur mon bouton incrément
  const handleClickIncrement = () => {
    // J'utilisation la fonction `dispatch` récupérer au dessus grâce à mon hook
    // J'utilise le action creator increment récupérer depuis mon reducer
    dispatch(increment());
  };

  // Au click sur mon bouton incrément by 5
  const handleClickIncrementBy = () => {
    // J'utilisation la fonction `dispatch` récupérer au dessus grâce à mon hook
    // J'utilise le action creator incrementBy récupérer depuis mon reducer
    dispatch(incrementBy(5));
  };

  return (
    <div>
      <button type="button" onClick={handleClickIncrement}>
        Incrément
      </button>
      <button type="button" onClick={handleClickIncrementBy}>
        Incrément by 5
      </button>
    </div>
  );
}
```
