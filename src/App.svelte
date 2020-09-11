<script>
  import { Router, Route, NotFound, redirect } from "./pager";

  import Login from "./pages/Login.svelte";
  import Home from "./pages/Home.svelte";
  import About from "./pages/About.svelte";
  import Profile from "./pages/Profile.svelte";

  const basePath = "/svelte-pagejs";

  const data = { foo: "bar", custom: true };

  const guard = (ctx, next) => {
    // check for example if user is authenticated
    if (true) {
      redirect("/login");
    } else {
      // go to the next callback in the chain
      next();
    }
  };
</script>

<style>
  main {
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  nav a {
    padding-right: 3rem;
  }
</style>

<main>
  <nav>
    <a href={basePath}>home</a>
    <a href="{basePath}/about">about</a>
    <a href="{basePath}/profile/joe">profile</a>
    <a href="{basePath}/news">news</a>
    <a href="{basePath}/login">login</a>
  </nav>

  <Router {basePath}>
    <Route path="/" component={Home} {data} />
    <Route path="/about" component={About} />
    <Route path="/login" component={Login} />
    <Route path="/profile/:username" let:params>
      <h2>Hello {params.username}!</h2>
      <p>Here is your profile</p>
    </Route>
    <Route path="/news" middleware={[guard]}>
      <h2>Latest News</h2>
      <p>Finally some good news!</p>
    </Route>
    <NotFound>
      <h2>Sorry. Page not found.</h2>
    </NotFound>
  </Router>
</main>
