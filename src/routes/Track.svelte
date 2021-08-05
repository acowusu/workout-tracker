<script>
import Counter from "../components/Counter.svelte";
import workouts from "../components/workouts.json";
export let id;
let workout = workouts[id];
</script>

<Counter
  id="{workout.id}"
  title="{workout.name}"
  description="{workout.description}"
  model="{workout.model}"
  image="{workout.icon}" />
