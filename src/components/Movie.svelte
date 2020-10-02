<script>
  import { Link } from "svelte-routing";
  import { pathImg, noImage } from "../helpers/service";

  export let img_url;
  export let title;
  export let movie_id;
</script>

<style>
  img {
    height: 250px;
    border-radius: 10px;
    transition: transform 0.2s;
    margin: 5px;
  }

  img:hover {
    transform: scale(1.1);
    cursor: pointer;
  }
</style>

<Link to={`movieDetail/${movie_id}`}>
  <img
    src={!img_url ? noImage : `${pathImg}${img_url}`}
    alt={`${title} Avatar`} />
</Link>
