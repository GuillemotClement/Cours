# Cours

## Utils

### Tree

Utilitaire pour la ligne de commande qui permet de lister les folder et fichier dans le terminal. C'est une amelioration de `ls`.
[Documentation](https://sourabhbajaj.com/mac-setup/iTerm/tree.html)

```shell
# installation sur mac
brew install tree

# lister les fichier
tree
```

### Github

pour fermer automatiquement une issues avec un commit/push

```shell
git commit -m "Ajoute la validation de formulaire - close #12"
```

#12 fait reference au numero de l'issue et close est un mot cle reconnu par github

# Workflow

## Docker

On commence par creer le projet React et on verifier qu'il fonctionne en local. On viens ensuite creer le fichier `dockerfile` dans le folder creer.
On definis les instruction necessaire pour creer le container charger de faire tourner l'applicatuion front end React.

On viens ensuite definir le `docker-compose` dans a la racine du projet. Ce fichier est charger d'orchestrer les differnts container du projet.

Pour que cela soit fonctionnel, il faut bien modfier la configuration de vite pour accedera au container depuis la machine,

```json
export default defineConfig({
	plugins: [react()],
	// A REAJOUTER si on lance le server dans un container docker
	// De base le server se lance sur localhost or nous on veut 0,0,0,0 pour pouvoir y accéder depuis l'extérieur du container !
	server: {
		host: "0.0.0.0",
	},
});
```

Une fois le dockerfile et le docker compose configurer on peut lancer le build du projet `docker compose up --build`

## Prisma

Pour le `database_url`, l'host correspond au nom du service dans le docker compose

Pour installer Prisma sur le projet Nest, il faut venir lancer les commandes depuis le container.

```shell
# installation de Prisma
$ npm install prisma --save-dev

# unit Prisma CLI => ajout du dossier Prisma pour definir les models
# venir ajouter le url_db dans le .env
$ npx prisma init

# une fois le model defis, on peut lancer la commande pour la migration
$ npx prisma migrate dev --name init

# installation du prisma client
$ npm install @prisma/client
# a chaque modif du model, on lance cette commande
$ npx prisma generate
```

## Nest

### Service Prisma

Une fois Prisma et le model de Db fait, il faut venir creer un nouvea un nouveau service `/src/prisma.service.ts`.
Ce service permet d'utiliser Prisma dans le projet Nest.

### Creation d'un module

Un module permet de gerer une ressource.

Pour generer un nouveau module avec les fichiers necessaire pour une ressource

```shell
$ nest g res
```

On viens ensuite indiquer le nom du module.

1. On definis l'entity
   Dans le fichier `<module_name>/entities`, on viens indiquer les differentes props de la ressource.

   Pour que cela fonctionne, il faut utiliser cette syntaxe.

```ts
import { Role } from "generated/prisma";

export class RoleEntity implements Role {
  // declaration des props de la ressource
  id: number;
  title: string;

  // ajoute du constructeur pour instancier la valeurs des props
  constructor(role: Role) {
    this.id = role.id;
    this.title = role.title;
  }
}
```

2. Definit le DTO
   cela permet de definir le typage lier aux methodes.

```ts
export class CreateRoleDto {
  title: string;
}
```

```ts
import { PartialType } from "@nestjs/mapped-types";
import { CreateRoleDto } from "./create-role.dto";

export class UpdateRoleDto extends PartialType(CreateRoleDto) {
  title: string;
}
```

3. On definis ensuite les methodes de cette ressource dans le service.

### Seed des data avec Prisma

On viens ajouter dans le dossier `src/prisma/seed.ts`. Dans ce fichier on viens definir les donnees a envoyer.

On ajoute ensuite cette ligne dans le fichier `package.json` :

```json
  "prisma": {
    "seed": "ts-node src/prisma/seed.ts"
  },
```

On viens ensuite lancer le seed depuis le container :

```shell
$ npx prisma db seed
```
