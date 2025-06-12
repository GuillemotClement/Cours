# Test

[Documentation Nest](https://docs.nestjs.com/fundamentals/testing)

Nest utilise `Jest` et `SuperTest` mais il est possible d'utiliser la solution que l'on souhaite.

On peut venir tester plusieurs aspects de l'application pour s'assurer du bon fonctionnement.

1. Controller
   Test des routes et methode d'un controller pour s'assure que les reponses sont correct.
2. Service
   Les services contiennent la logique metier de l'application. On peut venir tester les methodes pour s'assurer de leur bon fonctionnement, independament des controlleurs.

3. Modules
   Les modules sont des conteneurs qui regroupent des controleurs, les services et d'autres composant. On peut venir tester l'ontegration des differents composants au sein d'un module.

4. Middleware
   Les middlewares sont des fonctions qui ont acces aux objet de requetes et de reponse. On peut tester les middleware pour s'assurer qu'il modifient correctement les requetes et les reponse.

5. Guards
   Les guards sont utiliser pour determiner si une requete doit etre traitee par un controlleur. On peut tester les gardes pour s'assure qu'ils autorisent ou refusent l'acces comme prevu.

6. Interceptors
   Les intercepteurrs sont utiliser pour interceter et modifier les requetes et les reponse . On peut tester les intercepteurs pour s'assuurer de leur bon fonctionnement

7. Pipes
   Les filtres d'exceptions sont utilsier pour transformer ou valider les donnees entrantes. On peut tester les pipes pour s'assure que la transformation et la validation est correct

8. Exception filters
   Les filtres sont utiliser pour gerer les exception lancee par l'application. On peut tester les filtres pour s'assure qu'ils gerent les exceptions comme prevu

9. BDD
   On peut tester l'integration avec la BDD pour s'assurer que les operations de lecture et d'ecriture fonctionne bien

10. E2E
    Permet de tester l'ensemble de l'application, y compris entre les differents composants et les services externe. On peut utilise `SuperTest` pour envoyer des requete HTTP a l'applcaton pour verifier les reponses

## Installation

```shell
$ npm i --save-dev @nestjs/testing
```

## Test d'un controlleur

Pour tester le controller dans un module. on viens ecrire les tests dans le fichier `<name_module>.controller.spec.ts`. Le fichier doit etre placer dans le dossier qui contient le fichier du controlle associer.
Nest fournis le code pour preparer les tests.

```ts
// permet de creer des modules de tests
import { Test, TestingModule } from "@nestjs/testing";
// importation du controlle que l'on souhaite tester
import { RoleController } from "./role.controller";
// importation du fichier qui contient les services que l'on vas tester => contient les methodes
import { RoleService } from "./role.service";

// fonction qui permet de regrouper les tests liee. Ici on test le RoleController
describe("RoleController", () => {
  // variable qui stocke une instance du controller
  let controller: RoleController;

  // configuration avant chaque tests
  // beforeEach est une fonction qui s'execute avant chaque test dans le bloc
  beforeEach(async () => {
    // Test.createTestingModule => utiliser pour creer un module de test
    // prend un objet de configuration qui specifie les controlleurs et les services a inclure dans le module
    const module: TestingModule = await Test.createTestingModule({
      // on inclue le controller RoleController
      controllers: [RoleController],
      // on inclue le service RoleService. Permet d'instancier le controlleur avec toutes ces dependances
      providers: [RoleService],
      // compile() compile le module de test et retourne une promesse qui resout en un `TestingModule`
    }).compile();

    // module.get => cette  methode pour obtenir une instance de RoleController a partir du module de test. L'instance est stockee dans la variable controller pour etre utiliser dans les tests
    controller = module.get<RoleController>(RoleController);
  });

  // test de la definition du controller. Permet de verifier si la variable controlle est definis. Si definis le test passe.
  it("should be defined", () => {
    expect(controller).toBeDefined();
  });
});
```
