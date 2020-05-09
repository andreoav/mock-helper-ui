<script>
  import { onMount } from "svelte";
  import { slide } from "svelte/transition";

  let endpoints = [];
  let filter = { query: "", method: "ALL" };

  $: availableEndpoints = endpoints.filter((endpoint) => {
    if (filter.method === "ALL") {
      return true;
    }
    return endpoint.method === filter.method;
  });

  onMount(async function () {
    const response = await fetch("http://localhost:9000/endpoints");
    endpoints = await response.json();
  });

  function setMethodFilter(method) {
    return () => (filter.method = method);
  }
</script>

<div>
  <div class="columns">
    <div class="column">
      <div class="field has-addons">
        <p class="control is-expanded">
          <input
            type="text"
            class="input is-large"
            placeholder="https://petstore.swagger.io/v2/swagger.json" />
        </p>
        <div class="control">
          <button type="submit" class="button is-primary is-large">Go</button>
        </div>
      </div>
    </div>
  </div>
  <div class="columns">
    <div class="column is-5">
      <nav class="panel is-warning">
        <p class="panel-heading">Endpoints</p>
        <div class="panel-block">
          <p class="control has-icons-left">
            <input
              type="text"
              class="input"
              placeholder="Search"
              bind:value={filter.query} />
            <span class="icon is-left">
              <i class="fas fa-search" aria-hidden="true" />
            </span>
          </p>
        </div>
        <p class="panel-tabs">
          <a on:click|preventDefault={setMethodFilter('ALL')}>All</a>
          <a on:click|preventDefault={setMethodFilter('GET')}>GET</a>
          <a on:click|preventDefault={setMethodFilter('POST')}>POST</a>
          <a on:click|preventDefault={setMethodFilter('PUT')}>PUT</a>
          <a on:click|preventDefault={setMethodFilter('PATCH')}>PATCH</a>
          <a on:click|preventDefault={setMethodFilter('DELETE')}>DELETE</a>
        </p>
        {#each availableEndpoints as endpoint (endpoint.id)}
          <a class="panel-block" transition:slide>
            <span class="panel-icon">
              <i class="fas fa-book" aria-hidden="true" />
            </span>
            {endpoint.path}
          </a>
        {/each}
      </nav>
    </div>
  </div>
</div>
