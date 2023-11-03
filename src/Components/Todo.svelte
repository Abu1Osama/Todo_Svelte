
<script>
    import { onMount } from 'svelte';
    import { toast } from 'svelte-toastify';
    
    let tasks = [];
    let newTask = '';
  
    onMount(() => {
      const storedTasks = localStorage.getItem('tasks');
      if (storedTasks) {
        tasks = JSON.parse(storedTasks);
      }
    });
  
    $: totalTasks = tasks.length;
    $: completedTasks = tasks.filter(task => task.completed).length;
  
    function addTask() {
      if (newTask.trim() !== '') {
        const newTaskData = {
          text: newTask,
          completed: false,
        };
        tasks = [...tasks, newTaskData];
        toast.success('Task added successfully');
        localStorage.setItem('tasks', JSON.stringify(tasks));
      }
    }
  
    function markTaskCompleted(index) {
      const updatedTasks = [...tasks];
      updatedTasks[index].completed = true;
      tasks = updatedTasks;
      toast.success('Task marked as completed');
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }
  
    function markTaskPending(index) {
      const updatedTasks = [...tasks];
      updatedTasks[index].completed = false;
      tasks = updatedTasks;
      toast.success('Task marked as pending');
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }
  
    function removeTask(index) {
      const updatedTasks = [...tasks];
      updatedTasks.splice(index, 1);
      tasks = updatedTasks;
      toast.error('Task deleted');
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }
  </script>


<div class="main-container-todo">
    <div class="todo-container">
      <h2 class="todo">Todo List</h2>
      <div class="data">
        {#if totalTasks === 0}
          <p>No tasks added yet!</p>
        {:else}
          <p class="task-count">
            <span>Total tasks:</span> {totalTasks}
          </p>
          <p class="completed-count">
            <span>Completed tasks:</span> {completedTasks}
          </p>
        {/if}
      </div>
  
      <div class="input-container">
        <input type="text" bind:value={newTask} placeholder="Add a new task" class="task-input" />
        <button on:click={addTask} class="add-button">Add Task</button>
      </div>
    </div>
  
    {#if totalTasks === 0}
      <p class="no-tasks-message">No tasks added yet!</p>
    {:else}
      <ul class="task-list">
        {#each tasks as task, index}
          <li key={index}>
            <div class="task-list-data">
              <h3>Name:</h3>
              <h3 class="task-text">{task.text}</h3>
            </div>
            <div class="button-container">
              <h3>Status:</h3>
              {#if !task.completed}
                <button class="toggle pending" on:click={() => markTaskCompleted(index)}>Pending</button>
              {:else}
                <button class="toggle completed" on:click={() => markTaskPending(index)}>Completed</button>
              {/if}
              <button class="remove" on:click={() => removeTask(index)}>Remove</button>
            </div>
          </li>
        {/each}
      </ul>
    {/if}
  </div>


  <style>
    :root {
  --bg1:#29AAE1;
  --bg2:#00C893;
  --bg3:#1a161f;
  --bg4:#528070;
  --bg5:#3d404d;
  --bg6:#0f0f0f;
  --bg7:#390554;
 }
    .todo-container {
    padding: 20px;
    background-color: var(--bg7);
    border-radius: 4px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
    color: white;
  }
  
  .todo {
    text-align: center;
    display: flex;
    justify-content: center;
    gap: 5px;
   
  }
  /* .todo span img{
    width: 30px;
  } */
  
  .input-container {
    display: flex;
    margin-bottom: 10px;
    flex-direction: column;
    gap: 10px;
  }
  
  .task-input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    outline: none;
  }
  
  .add-button {
    padding: 5px 10px;
    background-color: var(--bg5);
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
  }
  
  .task-list {
    list-style-type: none;
    padding: 0;
    flex-grow: 1;
    height: max-content;
    display: grid;
    gap: 10px;
    transition: max-height 0.3s ease-in-out;
  }
  
  .task-list li,.no-tasks-message {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    padding: 10px;
    background-color: var(--bg5);
    border-radius: 4px;
    box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.2);
    gap: 10px;
    color: white;
  }
  
  .button-container {
    display: flex;
    gap: 10px;
  }
  
  .button-container button {
    border: transparent;
    cursor: pointer;
    padding: 5px 10px;
    border-radius: 8px;
  }
  
  .remove {
    background: tomato;
    color: white;
    font-size: 12px;
  }
  
  /* .remove>img {
    height: 20px;
  } */
  
  .task-count,
  .completed-count {
    font-size: 14px;
    font-weight: bold;
  }
  
  .completed {
    color: green;
  }
  
  .task-list-data {
    display: grid;
    grid-template-columns: auto auto;
    gap: 10px;
    width: 200px;
    justify-content: flex-start;
  }
  
  .data {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    text-align: center;
    flex-wrap: wrap;
  }
  
  .data p {
    font-size: 18px;
  }
  
  .data span {
    margin-right: 5px;
  }
  
  .toggle {
    padding: 5px 10px;
    color: #fff;
    border: none;
    cursor: pointer;
    font-size: 12px;
  }
  
  .toggle.completed {
    background-color: var(--bg2);
  }
  
  .toggle.pending {
    background-color: var(--bg1);
    color: white;
  }
  
  .main-container-todo {
    padding: 10px;
    display: flex;
    flex-direction: column;
    background-color: #ecfcff;
    width: 40%;
    margin: auto;
    justify-content: center;
  }
  
  .task-text {
    display: inline-block;
    max-width: 200px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  
  @media screen and (max-width: 600px) {
    .data p {
      flex-grow: 1;
      font-size: 18px;
      margin-top: 10px;
    }
    .main-container-todo {
 width: 100%;
    }
    .task-list {
      align-items: center;
      justify-content: center;
      grid-template-columns: 100%;
      padding: 10px 0;
      flex-grow: 1;
    }
  
    .task-list-data {
      width: 100%;
    }
  }
  
  </style>