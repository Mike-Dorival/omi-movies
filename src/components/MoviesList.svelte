<script>
  import { onMount } from "svelte";
  import Movie from "./Movie.svelte";

  import { token } from "../helpers/service";

  let items = [];
  let selectMoviesOrSeries = "";
  let query = "";

  const movies_url = `https://api.themoviedb.org/3/movie/popular?api_key=${token}&language=fr-FR&page=1`;
  const series_url = `https://api.themoviedb.org/3/tv/popular?api_key=${token}&language=fr-FR&page=1`;

  onMount(() => {
    getItems();
  });

  function queryFetch(url) {
    fetch(url)
      .then((res) => res.json())
      .then((data) => (items = data.results.slice(0, 18)))
      .catch((err) => console.log(err));
  }

  function selectCategory(e) {
    const value = e.target.innerHTML;
    selectMoviesOrSeries = value;

    if (value === "Films") {
      return queryFetch(movies_url);
    }

    if (value === "Séries") {
      return queryFetch(series_url);
    }
  }

  function selectFilter(e) {
    const value = e.target.value;

    if (value === "aucun") {
      if (selectMoviesOrSeries === "Séries") {
        return queryFetch(series_url);
      }

      queryFetch(movies_url);
    }

    if (value === "recents") {
      if (selectMoviesOrSeries === "Séries") {
        return (items = [...items].sort(
          (a, b) => new Date(b.first_air_date) - new Date(a.first_air_date)
        ));
      }
      items = [...items].sort(
        (a, b) => new Date(b.release_date) - new Date(a.release_date)
      );
    }

    if (value === "anciennes") {
      if (selectMoviesOrSeries === "Séries") {
        return (items = [...items].sort(
          (a, b) => new Date(a.first_air_date) - new Date(b.first_air_date)
        ));
      }
      items = [...items].sort(
        (a, b) => new Date(a.release_date) - new Date(b.release_date)
      );
    }

    if (value === "ordre") {
      console.log("items", items);
      if (selectMoviesOrSeries === "Séries") {
        return (items = [...items].sort((a, b) =>
          a.name.localeCompare(b.name)
        ));
      }
      items = [...items].sort((a, b) => a.title.localeCompare(b.title));
    }
  }

  function getItems() {
    const multi_search = `https://api.themoviedb.org/3/search/multi?api_key=${token}&language=fr-FR&query=${query}&include_adult=false`;

    if (query.length) {
      return queryFetch(multi_search);
    }

    queryFetch(movies_url);
  }
</script>

<style>
  h1 {
    color: #f4f4f4;
    text-align: center;
    font-family: "Open Sans", sans-serif;
    width: 100%;
  }

  p {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
    font-size: 40px;
    margin-top: 80px;
    min-width: 37%;
    font-weight: 900;
  }

  form {
    min-width: 33%;
  }

  select > option {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
  }

  select {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
    border-radius: 5px;
  }

  .moviesList-container {
    margin-left: 150px;
    margin-right: 150px;
  }

  .search {
    display: flex;
    justify-content: space-between;
    margin-top: 40px;
    color: #fff;
  }

  .searchTerm {
    min-width: 100%;
    border-right: none;
    padding: 5px;
    border-radius: 10px;
    outline: none;
    color: #f4f4f4;
    background-color: #f4f4f4;
  }

  .searchTerm:focus {
    color: #101010;
  }

  .moviesList-filter {
    display: flex;
  }

  .moviesList-filter h3 {
    font-family: "Open Sans", sans-serif;
    font-weight: 300;
    color: #f4f4f4;
    margin-left: 50px;
    font-size: 15px;
    cursor: pointer;
    transition-duration: 0.3s;
    padding: 5px 5px 0px;
    border-radius: 5px;
  }

  .moviesList-filter h3:hover {
    background-color: #fbdb16;
    color: #101010;
  }

  .moviesList-center-img {
    margin-top: 3%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding: 5px;
  }
</style>

<div class="moviesList-container">
  <p>
    Bienvenue sur omi,
    <br />
    votre catalogue de films,<br />
    séries, acteurs...
  </p>

  <div class="search">
    <form on:submit|preventDefault={getItems}>
      <input
        type="text"
        bind:value={query}
        class="searchTerm"
        placeholder="Rechercher un film, une série..." />
    </form>

    <select on:click={selectFilter}>
      <option value="aucun">Aucun</option>
      <option value="recents">Date récentes</option>
      <option value="anciennes">Date anciennes</option>
      <option value="ordre">Par ordre alphabétique</option>
    </select>

    <div class="moviesList-filter">
      <h3 on:click={selectCategory}>Films</h3>
      <h3 on:click={selectCategory}>Séries</h3>
    </div>
  </div>

  <div class="moviesList-center-img">
    {#if items.length}
      {#each items as movie}
        <Movie
          img_url={movie.poster_path}
          title={movie.title}
          movie_id={movie.id} />
      {/each}
    {:else if items.length === 0}
      <h1>Oups pas trouvé =(</h1>
    {/if}
  </div>
</div>
