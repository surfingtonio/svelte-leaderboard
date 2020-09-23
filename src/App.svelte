<script>
  import { onMount } from "svelte";
  import Leaderboard from "./components/Leaderboard.svelte";

  let users = [];

  onMount(async () => {
    let res = await fetch("/users.json");
    let data = await res.json();
    users = data.users;
  });
</script>

<main>
  <Leaderboard {users} />
</main>
