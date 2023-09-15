# Houdini+Sveltekit Typegen Bug Repro

## FIRST

Thank you to the folks who maintain `houdini`, and also thank you very much for taking a moment to look at my reproduction. 🙇‍♂️

## The Problem

The type `houdini generate` does not add `+layout.gql` query types to its children's generated `./.$houdini/PageData` types. This is causing my `svelte-check` command to fail in CI/CD pipelines.

I've only been able to reproduce this in `+layout.gql` files within `[param]` routes.

The types DO seem to get generated AFTER the development server has been started. 

## Reproduction Steps

### Clone this repo & Install Deps

```sh
$ git clone ... 
$ pnpm i
```

### Simulate CI/CD pipeline command chain

I've made helper scripts called `clean` and `reproduce`:

```sh
# deletes the $houdini and .svelte-kit directories
$ pnpm run clean

#  basically runs my ci/cd pipeline commands in order
$ pnpm run reproduce
```

OR you can run them on your own:

```sh
# generate sveltekit types
$ pnpm svelte-kit sync

# generate houdini types
$ pnpm houdini generate

# run type checking on svelte files
$ pnpm svelte-check
```

### Note the error emitted from `svelte-check`

```sh
houdini-layout-issue-repro/src/routes/[id]/+page.svelte:6:11
Error: Property 'FilmLayoutQuery' does not exist on type '{ RootLayoutQuery: RootLayoutQueryStore; }'. (ts)

    $: ({ FilmLayoutQuery } = data);
</script>
```

## My Workaround

I've just created a neighboring `+layout.ts` file, and loaded the document manually, according to the [Houdini Documentation](https://houdinigraphql.com/guides/working-with-graphql#manual-loads).

## Notes

### Dev still works

Once the development server is run, and the affected page is navigated to, houdini generates the type and type checking works as expected.

### Why do I expect +layout.gql data to appear in pages?

From [SvelteKit's documentation on Layout data](https://kit.svelte.dev/docs/load#layout-data):

> Data returned from layout load functions is available to child +layout.svelte components and the +page.svelte component as well as the layout that it 'belongs' to.

...and [their documentation on `$page.data`](https://kit.svelte.dev/docs/load#$page-data)...

> The +page.svelte component, and each +layout.svelte component above it, has access to its own data plus all the data from its parents.

### Why don't you run `houdini generate` before `svelte-kit sync`?

When I do that, I get the following errors from svelte










