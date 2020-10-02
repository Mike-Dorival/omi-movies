<script>
  import { onMount } from "svelte";
  import { Link } from "svelte-routing";

  import { token, pathImg, noImage } from "../helpers/service";

  export let id;

  let items = [];

  onMount(async () => {
    const movies_url = `https://api.themoviedb.org/3/movie/${id}?api_key=${token}&language=fr-FR`;
    const series_url = `https://api.themoviedb.org/3/tv/${id}?api_key=${token}&language=fr-FR`;

    try {
      const res = await fetch(movies_url);
      const data = await res.json();
      items = [...items, data];
    } catch (error) {
      console.log("error", error);
    }
  });

  function time_convert(num) {
    const hours = Math.floor(num / 60);
    const minutes = num % 60;
    return `${hours}h${minutes}`;
  }
</script>

<style>
  .content {
    display: flex;
    align-items: center;
    margin: 50px 150px 0px 150px;
  }

  img {
    height: 450px;
    border-radius: 10px;
    margin-right: 50px;
  }

  h1 {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
    font-size: 2.2rem;
  }

  span {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
    font-weight: 900;
  }

  .container :global(a) {
    color: #f4f4f4;
    margin-left: 150px;
    font-size: 1.5em;
    text-decoration: none;
  }

  .container :global(a:hover) {
    color: #fbdb16;
  }

  .container {
    margin-top: 100px;
  }

  p {
    color: #f4f4f4;
    font-family: "Open Sans", sans-serif;
    font-size: 20px;
    margin-right: 10px;
  }

  .italic {
    font-style: italic;
    opacity: 0.7;
  }

  .center-genre {
    display: flex;
    margin-bottom: 20px;
  }
</style>

<div class="container">
  <Link to="/">&laquo; Retour</Link>

  <div class="content">
    {#each items as item}
      <img
        src={!item.poster_path ? noImage : `${pathImg}${item.poster_path}`}
        alt={`${item.title} Avatar`} />
      <div class="detail">
        <h1>{item.title}</h1>
        <p>
          <span>Date de sortie :</span>
          {item.release_date.split('-').reverse().join('/')}
        </p>
        <div class="center-genre">
          <p>Genres :</p>
          {#each item.genres as genre}
            <p>{genre.name},</p>
          {/each}
        </div>
        <p class="italic">{item.tagline ? item.tagline : 'N/A'}</p>
        <p>
          <span>Synopsis</span>
          <br />
          {item.overview ? item.overview : 'N/A'}
        </p>
        <p><span>Durée : </span> {time_convert(item.runtime)}</p>
      </div>
    {/each}
  </div>
</div>
