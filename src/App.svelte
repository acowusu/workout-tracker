<style>
</style>

<script>
import Button from "./components/Button.svelte";
import Navbar from "./components/Navbar.svelte";
import "bootstrap/dist/css/bootstrap.css";
import Footer from "./components/Footer.svelte";
import Activities from "./components/Activities.svelte";
import Home from "./routes/Home.svelte";
import Track from "./routes/Track.svelte";

import { Router, Link, Route } from "svelte-routing";
let url = "";
</script>

<Router url="{url}">
  <Navbar />

  <div class="container">
    <Route path="track/:id" let:params>
      <Track id="{params.id}" />
    </Route>

    <Route path="/"><Home /></Route>
  </div>
  <Footer />
</Router>
