<script>
  import Note from './Note.svelte';
  import { onMount } from "svelte";

  let notes = [];
  let title = "";
  let content = "";

  onMount(() => {
    const savedNotes = localStorage.getItem('notes');
    if (savedNotes) {
      notes = JSON.parse(savedNotes);
    }
  });

  function saveNotes() {
    localStorage.setItem('notes', JSON.stringify(notes));
  }

  function addNote() {
    if (title || content) {
      notes = [...notes, { id: Date.now(), title, content, color: "#fff", pinned: false }];
      title = "";
      content = "";
      saveNotes();
    }
  }

  function deleteNote(id) {
    notes = notes.filter(note => note.id !== id);
    saveNotes();
  }

  function updateNote(updatedNote) {
    notes = notes.map(note => note.id === updatedNote.id ? updatedNote : note);
    saveNotes();
  }

  function togglePin(note) {
    note.pinned = !note.pinned;
    updateNote(note);
  }

  function changeColor(note, color) {
    note.color = color;
    updateNote(note);
  }
</script>

<div class="app">
  <div class="new-note">
    <input type="text" bind:value={title} placeholder="Title" />
    <textarea bind:value={content} placeholder="Take a note..."></textarea>
    <button on:click={addNote}>Add Note</button>
  </div>

  <div class="notes-grid">
    {#each notes.sort((a, b) => b.pinned - a.pinned) as note}
      <Note {note} onDelete={deleteNote} onUpdate={updateNote} onPinToggle={togglePin} onColorChange={changeColor} />
    {/each}
  </div>
</div>

<style>
  .app {
    padding: 20px;
    max-width: 900px;
    margin: auto;
  }

  .new-note {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .new-note input,
  .new-note textarea {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }

  .notes-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
  }
</style>