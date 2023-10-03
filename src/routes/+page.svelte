<!-- src/App.svelte -->
<script>
  import '../css/reset.css';
  import '../css/global.css';
  import Posts from "../components/Posts.svelte";
</script>

<main class="main">
  <h1>Headless CMS with Svelte</h1>
  <Posts />
</main>

<style>
  h1 {
    font-weight: 700;
    font-size: 3.5rem;
    color: var(--white);
    text-align: center;
    text-shadow: 0 3px 10px var(--black);
    margin-bottom: 2em;
  }

  .main {
    padding: 100px 0;
  }
</style>
