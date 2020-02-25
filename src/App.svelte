<script>
  import LazyComponent from './LazyComponent.svelte';
</script>

<style>
  :global(body, html)
  {
    margin: 0;
  }
  :global(section)
  {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
</style>

<main>
  <section style="background: cadetblue">A</section>
  <section style="background: mediumpurple">B</section>
  <LazyComponent src="/build/ExtraPage.mjs" options={{ threshold: 0.5 }}>
    <section style="background: lightyellow">
      <div style="height: 50%">Loading...</div>
    </section>
  </LazyComponent>
  <LazyComponent src="/build/Missing.mjs" options={{ threshold: 0.5 }}>
    <section style="background: lightyellow">
      <div style="height: 50%">Loading...</div>
    </section>
    <section slot="error" style="background: indianred" let:error>Error: {error}</section>
  </LazyComponent>
</main>
