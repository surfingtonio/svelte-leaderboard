<script>
  import { createEventDispatcher } from "svelte";

  export let user;
  const dispatch = createEventDispatcher();

</script>

<style>
  .leaderboard__item {
    align-items: center;
    display: flex;
    justify-content: space-between;
    margin: 0.5rem 0;
    padding: 0.25rem;
  }
  .leaderboard__user {
    text-align: left;
    width: 30%;
  }
  .leaderboard__score {
    text-align: right;
    width: 30%;
  }
  .leaderboard__buttons {
    display: flex;
    justify-content: center;
    width: 30%;
  }
  .leaderboard__button {
    background: #007bff;
    border: 1px solid #007bff;
    color: #fff;
    height: 2rem;
    line-height: 1;
    margin: 0 0.1rem;
    width: 2rem;
  }
  .leaderboard__button--disabled {
    background: #eee;
    border: 1px solid #ccc;
    color: #ccc;
  }
</style>

<div class="leaderboard__item">
  <span class="leaderboard__user">{user.name}</span>
  <div class="leaderboard__buttons">
    <button
      on:click={() => dispatch('step', { userId: user.id, step: 1 })}
      class="leaderboard__button">
      +
    </button>
    <button
      on:click={() => dispatch('step', { userId: user.id, step: -1 })}
      class="leaderboard__button"
      class:leaderboard__button--disabled={user.score < 1}
      disabled={user.score < 1}>
      -
    </button>
  </div>
  <span class="leaderboard__score">
    {user.score} {user.score === 1 ? 'point' : 'points'}
  </span>
</div>
