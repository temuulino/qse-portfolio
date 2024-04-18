<script>
  import ProjectCard from '../lib/components/project-card.svelte';
  import { onMount } from 'svelte';
  import { request } from 'graphql-request';
  import { authorsQuery, projectsQuery } from '$lib/graphql-queries'

  const endpoint = import.meta.env.VITE_GRAPHQL_API;

  let authors = [];
  let projects = [];

  onMount(async () => {
    try {
      const { authors: fetchedAuthors } = await request(endpoint, authorsQuery);
      const { projects: fetchedProjects } = await request(endpoint, projectsQuery);
      authors = fetchedAuthors;
      projects = fetchedProjects;
    } catch (error) {
      console.error(error);
    }
  });
</script>

<svelte:head>
  <title>Quantum Sparks Engineering</title>
</svelte:head>

<h1 class="font-bold text-center mb-20 text-5xl">
  Quantum Sparks Engineering
</h1>

{#each authors as { name, intro, picture: { url } }}
  <div class="flex mb-40 items-end">
    <div class="mr-6">
      <h2 class="text-3xl mb-4 font-bold tracking-wider">{name}</h2>
      <p class="text-xl mb-4">{intro}</p>
    </div>

    <img class="mask mask-squircle h-48" src={url} alt={name} />
  </div>
{/each}

<div
  class="grid gap-10 md:grid-cols-4 md:px-10 lg:grid-cols-6 lg:-mx-52"
>
  {#each projects as { name, slug, description, image }}
    <ProjectCard {name} {description} url={image[0].url} {slug} />
  {/each}
</div>
