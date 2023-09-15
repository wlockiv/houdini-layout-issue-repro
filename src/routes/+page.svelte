<script lang="ts">
    import type { PageData } from "./$houdini";

    export let data: PageData;

    $: ({ RootLayoutQuery } = data);
</script>

<h1>Bug Reproduction</h1>

{#if $RootLayoutQuery.data?.allFilms?.films}
    {#each $RootLayoutQuery.data.allFilms.films as film}
        <p><a href="/{film?.id}">{film?.title}</a></p>
    {/each}
{/if}
