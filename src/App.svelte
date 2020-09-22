<script>
  // List of users and their scores
  let users = [
    { id: 1, name: "Emma", score: 0 },
    { id: 2, name: "Noah", score: 0 },
    { id: 3, name: "James", score: 0 },
    { id: 4, name: "William", score: 0 },
    { id: 5, name: "Olivia", score: 0 }
  ];

  // A reactive list of sorted users
  $: sortedUsers = users.sort((a, b) =>
    // Basically, what this ternary does is
    // if the scores are equal, compare the names
    // otherwise compare scores.
    a.score === b.score
      ? a.name.localeCompare(b.name)
      : a.score > b.score
      ? -1
      : 1
  );

  // Instead of creating two separate functions
  // We'll handle the action in this one function
  function step(id, i) {
    // Find the user
    let user = users.find(user => user.id === id);
    // Increase/decrease score
    user.score += i;
    // Make sure score does not go below 0.
    // Although we disable the decrement button
    // when score is 0, it is still a good practice
    // to validate our data here.
    user.score = user.score < 0 ? 0 : user.score;
    // Assign changes to trigger reactivity
    users = [...users];
  }
</script>

<style>
  /* We're using BEM methodology http://getbem.com */
  .message {
    text-align: center;
  }
  .leaderboard {
    border: 1px solid #000;
    list-style: none;
    margin: 5rem auto 0;
    padding: 2rem;
    width: 20rem;
  }
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
    margin: 0 .1rem;
    width: 2rem;
  }
  .leaderboard__button--disabled {
    background: #eee;
    border: 1px solid #ccc;
    color: #ccc;
  }
</style>

<main>
  <ul class="leaderboard">
    {#if users.length > 0}
      {#each sortedUsers as user}
        <li class="leaderboard__item">
          <span class="leaderboard__user">{user.name}</span>
          <div class="leaderboard__buttons">
            <button on:click={step(user.id, 1)} class="leaderboard__button">
              +
            </button>
            <button
              on:click={step(user.id, -1)}
              class="leaderboard__button"
              class:leaderboard__button--disabled={user.score < 1}
              disabled={user.score < 1}>
              -
            </button>
          </div>
          <span class="leaderboard__score">
            {user.score} {user.score === 1 ? 'point' : 'points'}
          </span>
        </li>
      {/each}
    {:else}
      <li class="message">Nothing to show</li>
    {/if}
  </ul>
</main>
