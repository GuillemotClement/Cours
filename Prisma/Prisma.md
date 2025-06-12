# Prisma

## Installation

Il faut venir lancer les commandes d'installation de Prisma depuis le container Docker.
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

## Schema

Le fichier `prisma/schema.prisma` permet de definir les differentes table d'un projet.

```prisma
// permet de definir une nouvelle table
model Role {
  // definis la cle primaire
  id Int @id @default(autoincrement())
  title String @unique
  // indique une cle etrangere
  user User[]
  // permet de renommer le nom de la table dans la base de donnee
  @@map("role")
}

model User {
  id Int @id @default(autoincrement())
  username String @unique
  email String @unique
  password String
  image String
  createdAt DateTime @default(now())
  updatedAt DateTime? @updatedAt
  deletedAt DateTime?
  bookings Booking[]
  // permet de definir une cle primaire
  roleId Int
  // le field correpond au champs de la table qui est la cle etrangere
  // reference permet de definir le champs dans la table qui envoie cette cle etrangere
  role Role @relation(fields: [roleId], references: [id])
  comment Comment[]

  @@map("users")
}
```
