<script>
  import { flip } from "svelte/animate";
  import LeaderboardItem from "./LeaderboardItem.svelte";

  export let users = [];

  $: sortedUsers = users.sort((a, b) =>
    a.score === b.score
      ? a.name.localeCompare(b.name)
      : a.score > b.score
      ? -1
      : 1
  );

  function handleStep(event) {
    const { userId, step } = event.detail;
    let user = users.find(user => user.id === userId);
    user.score += step;
    user.score = user.score < 0 ? 0 : user.score;
    users = [...users];
  }
</script>

<style>
  .leaderboard {
    border: 1px solid #000;
    list-style: none;
    margin: 5rem auto 0;
    padding: 2rem;
    width: 20rem;
  }
  .message {
    text-align: center;
  }
</style>

<div class="leaderboard">
  {#each sortedUsers as user (user.id)}
    <div animate:flip={{ duration: 200 }}>
      <LeaderboardItem {user} on:step={handleStep} />
    </div>
  {:else}
    <p class="message">Nothing to show</p>
  {/each}
</div>
