<script>
  import { onMount } from "svelte";

  let name = "Seredata";
  $: tasks = [];

  let newItemText = "";

  let hideCompleted = true;
  $: filter = hideCompleted ? tasks.filter(t => !t.done) : tasks;

  function addNewTodo() {
    tasks = [...tasks, { action: newItemText, done: false }];
    storeData();
    newItemText = "";
  }

  function storeData() {
    localStorage.setItem("todos", JSON.stringify(tasks));
  }

  function deleteCompleted() {
    tasks = tasks.filter(t => !t.done);
    storeData();
  }

  onMount(() => {
    let data = localStorage.getItem("todos");
    if (data != null) {
      tasks = JSON.parse(data);
    }
  });
</script>

<style>

</style>

<h4 class="bg-primary text-white text-center p-2"><a class="text-white" href="https://seredata.com">{name}'s</a> To Do List</h4>
<div class="container-fluid p4">
  {#if filter.length == 0}
    <div class="row">
      <div class="col text-center">
        <b>Noting to do. Hurrah!</b>
      </div>
    </div>
  {:else}
    <div class="row">
      <div class="col font-weight-bold">Task</div>
      <div class="col-2 font-weight-bold">Done</div>
    </div>
  {/if}
</div>

{#each filter as t}
  <div class="row">
    <div class="col">{t.action}</div>
    <div class="col-2 text-center">
      <input type="checkbox" class="form-check-input" bind:checked={t.done} />
      {t.done}
    </div>
  </div>
{/each}

<div class="row py-2">
  <div class="col">
    <input type="text" bind:value={newItemText} class="form-control" />
  </div>
  <div class="col-2">
    <button class="btn btn-primary" on:click={addNewTodo}>Add</button>
  </div>
</div>

<div class="row bg-secondary py-2 mt-2 text-white">
  <div class="col text-center">
    <input
      type="checkbox"
      bind:checked={hideCompleted}
      class="form-check-input" />
    <label class="form-check-label font-weight-bold">
      Hide completed tasks
    </label>
  </div>
  <div class="col text-center">
    <button class="btn btn-sm btn-warning" on:click={deleteCompleted}>
      Delete Completed
    </button>
  </div>
</div>
