<script>
  import { Router, Link, Route } from "svelte-routing";
  import MoviesList from "./components/MoviesList.svelte";
  import MovieDetail from "./components/MovieDetail.svelte";

  export let url = "";
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400&family=Roboto:wght@400;900&display=swap");
  :global(*, body) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: none;
    background-color: #101010;
  }

  .container {
    height: 100vh;
  }

  nav {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    padding: 25px 150px;
  }

  nav :global(a:hover) {
    color: #fbdb16;
  }

  nav .logo {
    font-family: "Roboto", sans-serif;
    font-weight: 900;
    color: #fbdb16;
    font-size: 50px;
    cursor: default;
    user-select: none;
    padding: 0px;
  }

  nav .logo {
    flex: 1;
  }

  nav :global(a) {
    color: #f4f4f4;
    font-size: 15px;
    text-decoration: none;
    font-family: "Open Sans", sans-serif;
    font-weight: 300;
    transition: 0.4s;
    padding-left: 15px;
  }
</style>

<div class="container">
  <Router {url}>
    <nav>
      <a class="logo" href="##">omi</a>
      <Link to="/">Accueil</Link>
      <Link to="propos">À propos</Link>
      <Link to="contact">Contact</Link>
    </nav>
    <div>
      <Route path="/" component={MoviesList} />
      <Route path="movieDetail/:id" component={MovieDetail} />
    </div>
  </Router>
</div>
