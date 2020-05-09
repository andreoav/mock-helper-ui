<script>
  import { onMount } from "svelte";
  import { slide, fade } from "svelte/transition";

  import Search from "./Search/Search.svelte";

  let endpoints = [];
  let filter = { query: "", method: "ALL" };

  $: availableEndpoints = endpoints.filter((endpoint) => {
    if (filter.method === "ALL") {
      return true;
    }
    return endpoint.method === filter.method;
  });

  async function fetchEndpoints() {
    const response = await fetch("http://localhost:9000/endpoints");
    endpoints = await response.json();
    filter = { query: "", method: "ALL" };
  }

  function setMethodFilter(method) {
    return () => (filter.method = method);
  }
</script>

<div>
  <div class="columns">
    <div class="column">
      <Search onSearch={fetchEndpoints} />
    </div>
  </div>
  {#if endpoints.length > 0}
    <div class="columns">
      <div class="column is-4">
        <nav
          class="panel"
          transition:fade
          class:is-info={filter.method === 'GET'}
          class:is-success={filter.method === 'POST'}
          class:is-warning={filter.method === 'PUT'}
          class:is-danger={filter.method === 'DELETE'}
          class:is-primary={filter.method === 'PATCH'}>
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
      <div class="column">
        <nav class="panel" transition:fade>
          <p class="panel-heading">Selected endpoint</p>
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
  {/if}

</div>
