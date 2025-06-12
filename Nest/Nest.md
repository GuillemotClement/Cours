# Nest

## Service Prisma

Une fois Prisma et le model de Db fait, il faut venir creer un nouvea un nouveau service `/src/prisma.service.ts`.
Ce service permet d'utiliser Prisma dans le projet Nest.

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

## DTO

Le DTO est utiliser pour typer les donnees attendu de la requete. Il permet simplement de definir ce qui doit etre fournis dans la requete provenant du front, mais il n'est pas utiliser pour typer ce qui sera ensuite forunis en BDD dans le service traitant la requete.
c'est egalement ici que l'on effectue la validation des donnees fournis par le requetes.

```ts
import { IsEmail, IsNotEmpty, IsOptional, IsString, MinLength } from "class-validator";

export class CreateUserDto {
  @IsNotEmpty()
  @IsString()
  username: string;

  @IsNotEmpty()
  @IsEmail()
  email: string;

  @IsNotEmpty()
  @MinLength(6)
  password: string;

  @IsOptional()
  @IsString()
  image?: string;
}
```

## Controller

C'est lui qui se charge de recuperer les donnees fournis par la requete.

```ts
// user.controller.ts
@Post()
create(@Body() createUserDto: CreateUserDto) {
  return this.userService.create(createUserDto);
}
```

## Service

C'est lui qui se charge de faire le traitement de la requete. Exemple verifier si j'ai bien recuperer une image d'une requete de creation d'un nouvel utilisateur.

```ts
async create(createUserDto: CreateUserDto) {
  // on viens recuperer l'image depuis le DTO
  const image = createUserDto.image ?? 'https://picsum.photos/200'; // image par défaut
  // on envoie les donnes dans la BDD pour la creation
  const user = await this.prisma.user.create({
    data: {
      ...createUserDto,
      image,
      password: await this.hashPassword(createUserDto.password),
      roleId: 1, // exemple
    },
  });
  // on retourne le nouvel utilisateur qui a ete creer
  return new UserEntity(user);
}
```

## Module

Pour utiliser un service dans un autre service, on viens exporter le service souhaiter :
Cela permet de rendre disponible des methodes dispo dans un service

```ts
//users/users.module.ts
import { Module } from "@nestjs/common";
import { UsersService } from "./users.service";
import { UsersController } from "./users.controller";
import { PrismaService } from "src/prisma/prisma.service";

@Module({
  controllers: [UsersController],
  providers: [UsersService, PrismaService],
  exports: [UsersService], // on exporte le module pour le rendre utilisable ailleurs
})
export class UsersModule {}
```

Dans le service qui utilise le service exporter, on viens ensuite importer

```ts
//src/auth/auth.module.ts
import { Module } from "@nestjs/common";
import { AuthController } from "./auth.controller";
import { AuthService } from "./auth.service";
import { UsersModule } from "../users/users.module";

@Module({
  imports: [UsersModule], // on importe le service
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```
