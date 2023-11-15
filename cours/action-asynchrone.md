# Action Asynchrone

Les actions classiques avec redux n'ont qu'un seul état. C'est à dire que l'action est déclenchée, le reducer est appelé et le state est modifié.

Mais dans notre développement il nous arrive souvent de devoir faire des actions asynchrones. Par exemple, lorsqu'on veut récupérer des données depuis une API.

Dans ce cas notre action à besoin de gérer plusieurs état :

- En cours (pending)
- Réussi (fullfilled)
- Echoué (rejected)

## createAsyncThunk

`createAsyncThunk` permet de créer une action asynchrone, cad une action avec 3 états.

### Exemple

```ts
const monAction = createAsyncThunk('leNomDe/MonAction', async () => {});
```

Côté reducer on va pouvoir gérer la modification des données en fonction des 3 états de notre requête.

Les 3 cas à gérer seront

`monAction.pending` : Lorsque la requête est en cours
`monAction.fulfilled` : Lorsque la requête est réussi
`monAction.rejected` : Lorsque la requête est en erreur

```ts
const monSlice = createSlice({
  name: 'monSlice',
  initialState: {
    data: [],
    loading: false,
    error: null,
  },
  reducers: {},
  // La gestion des actions asynchrone se fait dans la propriété extraReducers
  extraReducers(builder) {
    builder
      // Lorsque mon action viens d'être lancer
      .addCase(monAction.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      // Lorsque mon action est réussi
      .addCase(monAction.fulfilled, (state, action) => {
        // j'arrête l'affichage du loader qui se baserai sur ma variable loading
        state.loading = false;
        // je modifie mon state avec les données récupérées
        state.data = action.payload;
      })
      // Lorsque mon action est en erreur
      .addCase(monAction.rejected, (state) => {
        // j'arrête l'affichage du loader qui se baserai sur ma variable loading
        state.loading = false;
        // J'afficher un message d'erreur
        state.error = "Oups, quelque chose s'est mal passé";
      });
  },
});
```

### Exemple de createAsyncThunk

#### Création de mon action

```ts
const fetchCategories = createAsyncThunk(
  'fetchCategories',# Redux Toolkit (RTK)

Redux Toolkit est une librairie qui permet d'utiliser redux de manière plus diriger.
Le but est de nous aider à bien écrire notre redux, éviter les erreurs et les répétitions.

## Installation

```bash
pnpm install @reduxjs/toolkit
```

## Configuration du store

Le store va être l'endroit où seront stocker les données de notre application. SSOT (Single Source Of Truth), source unique de vériter (un peu comme l'idée derrière une BDD).

```ts
// ./src/store/index.ts
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({
  // Je vais lui passé des reducers.
  // C'est eux qui s'occuperont de traduire les intentions en nouvelles données.
  reducer: {},
});
export default store;
```

## Création d'un reducer

Le reducer est une fonction qui va s'occuper de traduire les intentions en nouvelles données.
C'est également lui qui retournera la première les données initials à enregistrer.

[Documentation](https://redux-toolkit.js.org/api/createslice#parameters)

```ts
// ./src/store/reducers/counter.ts
import { createSlice } from '@reduxjs/toolkit';

type CounterState = {
  count: number;
};

// Je créer une variable initialState qui sera passé en paramètre du createSlice
// Je lui précise un type pour être précis sur les données qu'il contient.
const initialState: CounterState = {
  count: 0,
};

// le createSlice va nous permettre de créer un reducer et les actions créations automatiquement.
const counterSlice = createSlice({
  // Le nom du slice, il sera préfixer devant le nom de nos actions `counter/${monAction}`
  name: 'counter',
  // Les données initials
  initialState,
  // Puis on va lister les actions qui sera gérer par le reducer
  reducers: {
    // Ici on va retouver des fonctions qui seront exécuter lorsqu'on dispatchera une action
    // Équivalent du switch case dans un reducer sans toolkit.
    increment(state) {
      // Je peu modifier les données du state directement car redux toolkit utilise immer
      // Qui rend les données immutable mais me permet de faire comme si j'avais le droit de les modifier.
      state.count += 1;
    },
  },
});

// Je met à disposition le reducer et les actions créations automatiquement.
export const { increment } = counterSlice.actions;
export default counterSlice.reducer;
```

## Ajout du reducer au store

```ts
// ./src/store/index.ts
import { configureStore } from '@reduxjs/toolkit';
// J'importe mon reducer
import counterReducer from './reducers/counter';

const store = configureStore({
  reducer: {
    // Je renseigne mon reducer
    // Le nom de propriété `counter` sera l'emplacement des données dans le store que mon reducer gère.
    counter: counterReducer,
  },
});
export default store;
```

## Action avec paramètre

```ts
// ./src/store/reducers/counter.ts
const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment(state) {
      state.count += 1;
    },
    // J'utilise PayloadAction pour spécifier le type de la propriété `payload` de l'action
    incrementBy(state, action: PayloadAction<number>) {
      // action.payload sera de type number
      state.count += action.payload;
    },
  },
});

// Je met à disposition le reducer et les actions créations automatiquement.
export const { increment, incrementBy } = counterSlice.actions;
export default counterSlice.reducer;
```

## Utilisation des actions creators

Avec redux toolkit, lors de la création du slice, les actions sont créées automatiquement.
On les exportes avec :

```ts
export const { increment, incrementBy } = counterSlice.actions;

increment(); // { type: 'counter/increment' }
incrementBy(5); // { type: 'counter/incrementBy', payload: 5 }
```

    // On fais notre requête API
    // `data` est la propriété retourner par axios contenant les données réponse de l'API
    // On en profite également pour typer le retour de notre requête
    type Category = {
      id: number;
      name: string;
      slug: string;
    };
    const { data } = await axios.get<Category[]>(
      `http://localhost:3001/categories/${slug}`
    );

    return data;
  }
);
```

#### Utilisation de l'action

```tsx
function Categories() {
  const slug = 'toto';
  const dispatch = useAppDispatch();

  useEffect(() => {
    // On emet notre intention asynchrone comme une intention classique
    dispatch(fetchCategories(slug));
  }, []);
}
```
