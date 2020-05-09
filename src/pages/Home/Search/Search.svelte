<script>
  let error;
  let serviceUrl = "";

  export let onSearch;

  function handleSearch() {
    if (serviceUrl) {
      error = false;
      return onSearch({ serviceUrl });
    }

    error = true;
  }
</script>

<form on:submit|preventDefault={handleSearch}>
  <div class="field has-addons">
    <p class="control is-expanded">
      <input
        type="text"
        class="input is-large"
        class:is-danger={error}
        placeholder="https://petstore.swagger.io/v2/swagger.json"
        bind:value={serviceUrl} />
    </p>
    <div class="control">
      <button type="submit" class="button is-primary is-large">Go</button>
    </div>
  </div>
</form>
