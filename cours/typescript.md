# TypeScript

## But

S'assurer que nos données correspondent à une définition de type.

Ma variable va contenir un object avec des propriétés dont name qui est une chaine de caractère

## Déclaration d'un contract / d'un type

```ts
type MonContrat = {
  id: number;
  name: string;
  isValid: boolean;
};

type ListeDeContrat = MonContrat[];
```

### Utilisation du type

```ts
// Utilisation en paramètre
function afficherLeNomDuContrat(contrat: MonContrat) {
  console.log(contrat.name);
}

// Sur une variable
const contrat: MonContrat = {
  id: 1,
  name: 'Mon contrat',
  isValid: true,
};
```

## Déclaration d'un type pour une fonction

```ts
const somme = (num1: number, num2: number) => {
  return num1 + num2;
};

// Le type de la fonction
// La différence entre la déclaration de la fonction et le type
// `const somme` => `type Somme`
// Après la `=>` on retrouve le type de retour de la fonction plutôt que le corps de la fonction
type Somme = (num1: number, num2: number) => number;
```

## Exemple de code

[typescript](https://stackblitz.com/edit/typescript-qhw1xj?file=index.ts)
[typescript react](https://stackblitz.com/edit/stackblitz-starters-acp1q5?description=React%20%20%20TypeScript%20starter%20project&file=src%2FApp.tsx&title=React%20Starter)
