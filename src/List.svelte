<script>
  let cats = [
    {id: 'J---aiyznGQ', name: 'Keyboard Cat'},
    {id: 'z_AbfPXTKms', name: 'Maru'},
    {id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat'},
  ]
</script>

<p>- List Component -</p>
<ul>
  {#each cats as cat}
    <li><a target="_blank" href="http://www.youtube.com/watch?{cat.id}">
      {cat.name}
    </a></li>
  {/each}
</ul>

<style>
    p {
        color: darkolivegreen;
        font-size: 1em;
        font-weight: 100;
    }
</style>
