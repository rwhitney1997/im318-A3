<script>
     let todoItem = $state('');
     let todoList = $state([]); //square brackets for array

function addItem(event) {
     event.preventDefault(); //when enter is clicked the focus remains on the submit form
     if (todoItem == '') { //refuses to take submit when field is empty
          return;
     }
     todoList.push(todoItem);
     todoItem = '';
}
$inspect(todoList);
</script>


<form onsubmit={addItem}>
<input type="text" bind:value={todoItem}>
<button type="submit">Add Event</button>
</form>

<ul>
     {#each todoList as item}
          <li>{item}</li>
     {/each}
</ul>


<style>

</style>