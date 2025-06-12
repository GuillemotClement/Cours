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
