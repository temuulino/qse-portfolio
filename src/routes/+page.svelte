<!-- import.meta.env.VITE_GRAPHQL_API -->
<script>
  import { onMount } from 'svelte';
  import { request, gql } from 'graphql-request';

  let data = null;
  const endpoint = import.meta.env.VITE_GRAPHQL_API;

  const query = gql`
    query GetProjects {
      projects {
        name
        slug
        description
        demo
        sourceCode
        image {
          url
        }
      }
    }
  `;

  onMount(async () => {
    try {
      data = await request(endpoint, query);
    } catch (error) {
      console.error(error);
    }
  });
</script>

<main>
  {#if data}
    <pre>{JSON.stringify(data, null, 2)}</pre>
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  pre {
    background-color: #f4f4f4;
    padding: 1rem;
    border-radius: 8px;
  }
</style>
