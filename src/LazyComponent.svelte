<script>
  import { onMount } from 'svelte';

  export let src;
  export let options = undefined;

  let intersectingTarget;
  let resolve;
  let reject;

  const importModule = new Promise((_resolve, _reject) =>
  {
    resolve = _resolve;
    reject = _reject;
  });

  function intersecting([entry], observer)
  {
    if (entry.isIntersecting)
    {
      observer.disconnect();

      import(src).then(module =>
      {
        observer.unobserve(intersectingTarget);
        resolve(module);
      }).catch(error =>
      {
        reject(error)
      });
    }
  }

  onMount(() =>
  {
    const observer = new IntersectionObserver(intersecting, options);
    observer.observe(intersectingTarget);
  });
</script>

{#await importModule}
  <div bind:this={intersectingTarget}>
    <slot>Loading....</slot>
  </div>
{:then module}
  <svelte:component this={module.default} />
{:catch error}
  <slot name="error" {error}>
    <p>Failed loading module: {error.message}</p>
  </slot>
{/await}
