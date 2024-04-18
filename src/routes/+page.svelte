<script>
  import ProjectCard from '../lib/components/project-card.svelte';
  import { onMount } from 'svelte';
  import { request, gql } from 'graphql-request';

  let projects = null;
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
      const { projects: fetchedProjects } = await request(endpoint, query);
      projects = fetchedProjects;
    } catch (error) {
      console.error(error);
    }
  });

</script>

<h1>
  Recent Projects By QSE
</h1>

<main>
  {#if projects}
    {#each projects as { name, slug, description, image }}
      <div>
        <ProjectCard {name} {description} url={image[0].url} {slug} />
      </div>
    {/each}
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  /* Add your CSS styles here */
</style>
