<!-- src/Posts.svelte -->
<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import Content from './Content.svelte';

  let posts = [];

  onMount(async () => {
    try {
      const endpoint = import.meta.env.VITE_PAYLOADCMS_API_ENDPOINT;

      if (!endpoint) {
        throw new Error('API endpoint not defined');
      }
      const response = await axios.get(`${endpoint}/posts`);
      posts = response.data.docs;

    } catch (error) {
      console.error("Error fetching data:", error);
    }
  });

  const formatDate = (date) => {
    const dateObject = new Date(date);
    const year = dateObject.getFullYear();
    const month = String(dateObject.getMonth() + 1).padStart(2, '0');
    const day = String(dateObject.getDate()).padStart(2, '0');

    return `${year}-${month}-${day}`;
  }
</script>

<main>
  {#if posts.length > 0}
    <div class="wrapper">
      {#each posts as post (post.id)}
        <article class="article">
          <h2 class="title">{post.title}</h2>
          <p class="details">Published by <em>{post.author.firstName}</em> <em>{post.author.lastName}</em> on <em>{formatDate(post.publishDate)}</em></p>
          <figure class="image">
            <img src={`http://localhost:3000${post.coverImage.url}`} alt={post.coverImage.alt} />
          </figure>
          <p class="excerpt">{post.excerpt}</p>
          <div class="content">
            {#if post.content.length > 0}
              <Content nodes={post.content} />
            {:else}
              <p>No content available.</p>
            {/if}
          </div>
        </article>
      {/each}
    </div>
  {:else}
    <p>No posts available.</p>
  {/if}
</main>

<style>
  .wrapper {
    max-width: clamp(300px, 100%, 700px);
    margin: 5rem auto;
  }

  .article + .article {
    margin-top: 4rem;
    padding-top: 3rem;
    border-top: 1px solid #666;
  }

  .title {
    font-size: 2.25rem;
    font-weight: bold;
    margin-bottom: 0.5em;
    color: #fff;
  }

  .image {
    position: relative;
    width: 100%;
    height: 300px;
  }

  .image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .excerpt {
    font-size: 1.25rem;
    line-height: 1.5;
    margin-bottom: 1em;
    margin-top: 0.75em;
    font-style: italic;
    color: #fff;
  }

  .details {
    font-size: 0.875rem;
    color: #bbb;
    margin-bottom: 1em;
  }
</style>
