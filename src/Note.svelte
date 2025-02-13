<script>
  export let note;
  export let onDelete;
  export let onUpdate;
  export let onPinToggle;
  export let onColorChange;

  function handleUpdate() {
    onUpdate({ ...note });
  }
</script>

<div class="note" style="background-color: {note.color}">
  <div class="note-actions">
    <button on:click={onPinToggle}>{note.pinned ? '📌' : '📍'}</button>
    <button on:click={() => onDelete(note.id)}>🗑️</button>
  </div>
  <input type="text" bind:value={note.title} on:blur={handleUpdate} placeholder="Title"/>
  <textarea bind:value={note.content} on:blur={handleUpdate} placeholder="Content"></textarea>
  <div class="color-palette">
    {#each ['#F28B82', '#FBBC04', '#FFF475', '#CCFF90', '#A7FFEB', '#CBF0F8', '#AECBFA'] as color}
      <button style="background-color: {color}" on:click={() => onColorChange(note, color)}></button>
    {/each}
  </div>
</div>

<style>
  .note {
    width: 200px;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    position: relative;
  }

  .note-actions {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  .note textarea, .note input {
    width: 100%;
    padding: 5px;
    resize: none;
    border: none;
    border-bottom: 1px solid #ddd;
    margin-bottom: 10px;
    outline: none;
  }

  .color-palette {
    display: flex;
    gap: 5px;
    justify-content: flex-end;
  }

  .color-palette button {
    width: 20px;
    height: 20px;
    border: none;
    border-radius: 50%;
    cursor: pointer;
  }
</style>