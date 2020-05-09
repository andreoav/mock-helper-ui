<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./pages/Home/Home.svelte";

  export let url = "";
</script>

<!-- App.svelte -->
<section class="section">
  <div class="container">
    <Router {url}>
      <Route path="/" component={Home} />
    </Router>
  </div>
</section>
