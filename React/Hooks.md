# Hook React

## useEffect

Ce hook permet d'executer du code a certain moment du cycle de vie d'un composant React.

On l'utilise pour :

- Appel API
- Interaction avec des services externes (websocket, log, envoie de mails)
- Mise en place de timer
- Acces au DOM reel (poser un ecouteur d'event, mettre un focus dans un input, modifier un titre)

### Syntaxe

`useEffect` prend deux parametres

- une callback qui contient le code de l'effet
- un tableau de dependance qui definis quand est executer la callback
  - `null` : callback executer a chaque rendu
  - `[]` : callback executer uniquement apres le premier render
  - `[dependance]` : callback executer apres le premier rendu + apres apres les rendus pour lequel la valeur `dependance` evolue

```ts
useEffect(
  // callback contenant le code de l'effet
  () => {
    // code de l'effet
  },
  // tableau de dependances
  []
);
```
