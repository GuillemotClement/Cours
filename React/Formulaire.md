# Formulaire

```ts
export default function Login() {
  // pour conserver la saisis de l'utilisateur en cas d'erreur
  const [username, setUsername] = useState<string>("");
  // si les credendial sont pas correct
  const [error, setError] = useState<string>("");
  // permet de rediriger si le login a reussis
  const navigate = useNavigate();

  const url = "http://localhost:3000/auth/login";
  // fonction qui gere la soumission du formulaire
  const login = async (formData: FormData) => {
    const username = formData.get("username");
    const password = formData.get("password");
    // on creer l'objet qui est ensuite fournis dans la requete
    const data = {
      username,
      password,
    };
    // on vient faire la requete POST pour envoyer les data au back end
    try {
      const response = await fetch(url, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      });
      // si l'auth echoue on viens set l'erreur qui est ensuite afficher dans le formulaire
      if (response.status == 401) {
        setError("Credential inconnu");
      }
      // si il y une erreur serveur
      if (!response.ok) {
        throw new Error("failed call the server");
      }
      // on recupere le token retourner par le back end
      const { access_token } = await response.json();
      // on redirige vers la page d'accueil
      navigate("/");
    } catch (err) {
      console.error(err);
    }
  };

  return (
    <div className='flex flex-col justify-center items-center h-screen'>
      // action permet de binder avec la methoque qui catch la soumission du formulaire
      <form action={login} className='border rounded shadow p-10'>
        <h2 className='text-2xl text-center'>Connexion</h2>
        <div className='mb-4'>
          <label htmlFor='username'>Username</label>
          // name permet de definir le nom du input pour le recuperer ensuite dans la methode qui catch
          la soumnission du formulaire // l'event onChange permet de gerer la valeur dans l'input
          <input
            type='text'
            id='username'
            className='input'
            name='username'
            value={username ?? ""}
            onChange={(e) => setUsername(e.target.value)}
          />
        </div>
        <div className='mb-2'>
          <label htmlFor='password'>Mot de passe</label>
          <input type='password' className='input' name='password' />
        </div>
        <div className='mb-4'>
          <Link to={"/register"} className='text-sm text-blue-500 hover:underline'>
            Creer un compte
          </Link>
        </div>
        <div className='flex justify-between px-5'>
          <Link to={"/"} className='btn btn-neutral'>
            Retour
          </Link>
          // button qui permet de declencher l'envoie du formulaire
          <button type='submit' className='btn btn-primary'>
            Connexion
          </button>
        </div>
        // affiche l'erreur de credential
        {error && <p className='text-red-500 text-sm mt-4 text-center'>{error}</p>}
      </form>
    </div>
  );
}
```
