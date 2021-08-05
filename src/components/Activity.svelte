<style>
.item {
  color: #313437;
  background-color: #fff;
}

.item p {
  color: #7d8285;
}

.item {
  padding-top: 50px;
  min-height: 425px;
  text-align: center;
}

.item .name {
  font-weight: bold;
  font-size: 16px;
  margin-top: 20px;
  color: inherit;
}

.item .description {
  font-size: 14px;
  margin-top: 15px;
  margin-bottom: 0;
}
</style>

<script>
export let id;
export let image = "/assets/1.jpg";
export let title = "Title";
export let description = "description";
export let enabled;
import { Router, Link, Route } from "svelte-routing";
</script>

<div class="col-sm-6 col-md-3 item">
  <Link to="{'track/' + id}">
    <img class="img-fluid" src="{image}" alt="{title}" /></Link>
  <h3 class="name">{title}</h3>
  <p class="description">{description}</p>
  {#if enabled}
    <Link to="{'track/' + id}" class="btn mt-3 btn-primary">Start</Link>
  {:else}
    <button type="button" class="btn mt-3 btn-outline-secondary" disabled
      >Coming soon</button>
  {/if}
</div>
