# Fetching

## GET

Pour faire une appel API vers un serveur pour recuperer des donnes, on utilise le hook `useEffect` et `useSate`.

- `useState` : utiliser pour stocker les donnees recuperer depuis le serveur
- `useEffect` : c'est le hook qui permet de faire l'appel API au serveur

### Exemple

```ts
// on definis une interface pour typer les donnees recuperer
// on place les interface dans un dossier /src/@types/working.ts
export interface IWorking {
  id: number;
  title: string;
  image: string;
  localisation: string;
  createdAt: string;
  updatedAt: string;
  deletedAt: null;
}
```

Dans le composant qui permet de lister les differentes data recuperer on viens faire ce code

```ts
// importation des hook React
import { useEffect, useState } from "react";
// importation de l'interface
import type { IWorking } from "../../@types/working";
// importation du composant enfant
import CardCoworking from "./components/CardCoworking";

export default function ListCoworking() {
  // on definis le useState qui permet de conserver les datas
  const [listWorking, setListWorking] = useState<IWorking[]>([]);
  // on viens faire l'appel vers le serveur
  useEffect(() => {
    const fetchWorking = async () => {
      try {
        // on fait la requete
        const response = await fetch("http://localhost:3000/workings");
        // gestion d'erreur si le call echoue
        if (!response.ok) {
          throw new Error("Failed to fetch data");
        }
        // conversion du json en objet js
        const data: IWorking[] = await response.json();
        // on passe les donnees dans le useState
        setListWorking(data);
      } catch (err) {
        console.error(err);
      }
    };
    // on appelle la fonction pour faire la requete
    fetchWorking();
  }, []);

  return (
    <div className=''>
      <p>ListCoworking</p>
      <div className=''>
        // on map la liste pour passer les data dans le composant enfant
        {listWorking.map((working) => (
          <CardCoworking key={working.id} working={working} />
        ))}
      </div>
    </div>
  );
}
```

Dans le composant enfant on oeut ensuite traiter les donnees fournis par le composant parent

```ts
// importation de l'interface pour le typage
import type { IWorking } from "../../../@types/working";
// impoortation de la fonction pour traiter les coordonnees GPS
import { extractCoordGPS } from "../../../utils/helpers";

// creation de l'interfaace pour typer les props passer par le composant parent
interface CardCoworkingProps {
  working: IWorking;
}

// on type les props recus
export default function CardCoworking({ working }: CardCoworkingProps) {
  // destructurin]g pour recuperer les data provenant de l'objet passer par le composant parent
  const { title, image, localisation } = working;
  // destructuring pour recuperer les data retourner par la fonction
  const { lat, long } = extractCoordGPS(localisation);

  return (
    <div className=''>
      <h2>{title}</h2>
      <img src={image} alt='' />
      <ul>
        <li>Latitude : {lat}</li>
        <li>Longitude: {long}</li>
      </ul>
    </div>
  );
}
```
