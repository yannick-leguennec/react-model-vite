# Router

## But

Il nous permet de gérer les composants à afficher en fonction de l'URL.

- si `/` alors on affiche `Home`
- si `/about` alors on affiche `About`
  ...

## Installation de React Router DOM

```bash
pnpm install react-router-dom
```

[Voir la documentation](https://reactrouter.com/en/main)

## Configuration

### Configuration de nos routes

```tsx
// src/routes.tsx
import { createBrowserRouter } from 'react-router-dom';

export const router = createBrowserRouter([]);
```

#### Exemple avec 1 route

```tsx
// src/routes.tsx
import { createBrowserRouter } from 'react-router-dom';

export const router = createBrowserRouter([
  {
    // sur l'url `/`
    path: '/',
    // J'affiche le composant Home
    element: <Home />,
  },
]);
```

#### Exemple avec des routes imbriquées

```tsx
// src/routes.tsx
import { createBrowserRouter } from 'react-router-dom';

export const router = createBrowserRouter([
  {
    // sur l'url `/`
    path: '/',
    // J'affiche le composant Root si l'url commence par `/`
    // Le composant Root sera à afficher un layout (header, footer, ...)
    // Et on positionnera à l'intérieur l'endroit où l'on souhaitera afficher les routes enfants
    // Le positionnement se fera à l'aide du composant `Outlet`
    // Les enfants prendront la place de ce composant
    element: <Root />,
    // On définit les routes enfants / imbriquées
    // Les éléments mis dans les children seront afficher dans le composant `Outlet`
    children: [
      {
        path: '/',
        // J'affiche le composant Home
        element: <Home />,
      },
      {
        path: '/contact',
        // J'affiche le composant Contact
        element: <Contact />,
      },
    ],
  },
]);
```

```tsx
// src/routes/Root.tsx
function Root() {
  return (
    <>
      <header>
        <h1>Yeah men</h1>
      </header>
      <Outlet />
      <footer>Mon super footer</footer>
    </>
  );
}
```

#### Gérer les routes non trouvées / en erreur

```tsx
// src/routes.tsx
export const router = createBrowserRouter([
  {
    // sur l'url `/`
    path: '/',
    // J'affiche le composant Home
    element: <Home />,
    // On définit le composant à afficher en cas d'erreur
    // - si l'url ne correspond à aucune route
    // - si une erreur est levée dans un composant (throw new Error('Une erreur'))
    errorElement: <Error />,
  },
]);
```

#### Gérer les routes avec des paramètres

```tsx
// src/routes.tsx
export const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      {
        path: '/',
        element: <Home />,
      },
      {
        // Toutes les routes qui serai sous la forme `/product/1` ou `/product/2` ...
        path: '/product/:id',
        element: <Product />,
      },
    ],
  },
]);
```

Récupération du paramètre dans le composant

```tsx
// src/routes/Product.tsx
import { useParams } from 'react-router-dom';
function Product() {
  // On récupère le paramètre `id` dans l'objet `params`
  const { id } = useParams();
  return <h1>Produit avec l'identifiant {id}</h1>;
}
```

### Renseigner la config du router dans notre application

```tsx
// src/main.tsx
import { RouterProvider } from 'react-router-dom';
import { router } from './routes';

const root = ReactDOM.createRoot(document.getElementById('root')!);

root.render(<RouterProvider router={router} />);
```
