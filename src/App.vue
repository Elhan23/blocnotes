<template>
  <div class="notes-app">
    <div class="notes-list">
      <div
        v-for="(note, index) in notes"
        :key="index"
        class="note-item"
        @click="selectNote(index)"
        :class="{ 'selected': index === selectedNoteIndex }"
      >
        {{ note }}
        <span @click.stop="deleteNote(index)">X</span>
      </div>
    </div>
    <div class="note-editor">
      <textarea v-model="currentNote" placeholder="Добавить заметку..."></textarea>
      <div class="editor-buttons">
        <button @click="saveNote">Сохранить</button>
        <button @click="clearNote">Очистить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [],
      currentNote: '',
      selectedNoteIndex: null
    };
  },
  mounted() {
    this.loadNotes();
  },
  methods: {
    loadNotes() {
      const savedNotes = localStorage.getItem('notes');
      if (savedNotes) {
        this.notes = JSON.parse(savedNotes);
      }
    },
    saveNotes() {
      localStorage.setItem('notes', JSON.stringify(this.notes));
    },
    saveNote() {
      if (this.currentNote.trim()) {
        if (this.selectedNoteIndex !== null) {
          this.notes[this.selectedNoteIndex] = this.currentNote;
        } else {
          this.notes.push(this.currentNote);
        }
        this.saveNotes();
        this.clearNote();
      }
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
      this.saveNotes();
    },
    clearNote() {
      this.currentNote = '';
      this.selectedNoteIndex = null;
    },
    selectNote(index) {
      this.currentNote = this.notes[index];
      this.selectedNoteIndex = index;
    }
  }
};
</script>

<style>

body{
   background:powderblue;
}


.notes-app {
  display: flex;
  flex-direction: row;
}
.notes-list {
  width: 30%;
  padding: 20px;
}
.note-item {
  padding: 10px;
  cursor: pointer;
  border-bottom: 1px solid #ccc;
}
.note-item.selected {
  background-color: #f0f0f0;
}
.note-item span {
  float: right;
  color: red;
  cursor: pointer;
}
.note-editor {
  width: 70%;
  padding: 20px;
}
textarea {
  width: 100%;
  height: 200px;
  margin-bottom: 10px;
}
.editor-buttons button {
  margin-right: 10px;
}

 button:hover{
   color: cadetblue;
}
</style>
