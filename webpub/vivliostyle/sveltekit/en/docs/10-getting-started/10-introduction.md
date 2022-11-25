
# Introduction


## Before we begin

> SvelteKit is in release candidate phase for 1.0 while we address reported issues and add polish. If you get stuck, reach out for help in the [Discord chatroom](https://svelte.dev/chat).
>
> See the [migration guides](/docs/migrating) for help upgrading from Sapper.

## What is SvelteKit?

SvelteKit is a framework for building extremely high-performance web apps.

Building an app with all the modern best practices is fiendishly complicated. Those practices include [build optimizations](https://vitejs.dev/guide/features.html#build-optimizations), so that you load only the minimal required code; [offline support](../30-advanced/40-service-workers.html); [prefetching](../30-advanced/30-link-options.html#data-sveltekit-prefetch) pages before the user initiates navigation; and [configurable rendering](../20-core-concepts/40-page-options.html) that allows you to render your app [on the server](../60-appendix/30-glossary.html#ssr) or [in the browser](../60-appendix/30-glossary.html#csr-and-spa) at runtime or [at build-time](../60-appendix/30-glossary.html#prerendering). SvelteKit does all the boring stuff for you so that you can get on with the creative part.

It uses [Vite](https://vitejs.dev/) with a [Svelte plugin](https://github.com/sveltejs/vite-plugin-svelte) to provide a lightning-fast and feature-rich development experience with [Hot Module Replacement (HMR)](https://github.com/sveltejs/vite-plugin-svelte/blob/main/docs/config.md#hot), where changes to your code are reflected in the browser instantly.

You don't need to know Svelte to understand the rest of this guide, but it will help. In short, it's a UI framework that compiles your components to highly optimized vanilla JavaScript. Read the [introduction to Svelte blog post](https://svelte.dev/blog/svelte-3-rethinking-reactivity) and the [Svelte tutorial](https://svelte.dev/tutorial) to learn more.
<span style='float: footnote;'><a href="../../index.html#toc">Go to TOC</a></span>
