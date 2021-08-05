<script>
import Intro from "./Intro.svelte";
import workouts from "./workouts.json";
import Activity from "./Activity.svelte";
</script>

<Intro />

<div class="row activities">
  {#each workouts as { id, name, description, image, enabled }, i}
    <Activity
      id="{id}"
      title="{name}"
      description="{description}"
      enabled="{enabled}"
      image="{'/assets/' + image}" />
  {/each}
</div>
