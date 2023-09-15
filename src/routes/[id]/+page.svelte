<script lang="ts">
    import type { PageData } from "./$houdini";

    export let data: PageData;

    $: ({ FilmLayoutQuery } = data);
</script>

{#if $FilmLayoutQuery.data?.film}
    <h1>{$FilmLayoutQuery.data?.film?.title}</h1>
    <a href="/">Go Back Home</a>
    <h2>Opening Crawl</h2>
    <hr />
    {#if $FilmLayoutQuery.data.film.openingCrawl}
        <div style="max-width:400px">
            {#each $FilmLayoutQuery.data.film.openingCrawl.split("\r\n") as paragraph}
                <p>{paragraph}</p>
            {/each}
        </div>
    {/if}
{/if}
