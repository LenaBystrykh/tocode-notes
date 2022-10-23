<template>
    <Form @sendNote="displayNote" :tags="tags" :chosen="chosenTags" @changeTag="(value) => changeTag(value)"/>
    <div class="notes-list">
        <NoteItem 
            class="note-item"
            v-for="(note, index) in notes" 
            :key="index"
            :text="note.note"
            :chosenTags="note.tags"
            :allTags="tags"
            @delete="deleteNote(index)"
            @updateValue="(value) => changeNoteText(index, value)"
            @changeTag="(value) => changeNoteTag(index, value)" />
    </div>
</template>

<script>
import Form from '@/components/Notes/Form'
import NoteItem from '@/components/Notes/NoteItem'
export default {
    components: { Form, NoteItem },
    data() {
        return {
            notes: [
                {
                    note: 'Here you can manage your notes',
                    tags: ['study'],
                    type: 'text'
                },
                {
                    note: 'You can add one or several tags to your note or left it without tags',
                    tags: ['travel', 'sport'],
                    type: 'text'
                },
                {
                    note: 'You can change either text and tags or delete note',
                    tags: [],
                    type: 'text'
                }],
            tags: ['home', 'work', 'travel', 'study', 'pets', 'sport'],
            chosenTags: []
        }
    },
    mounted() {
        this.getNotes()
    },
    methods: {
        updateNotes(notesList) {
            localStorage.setItem('notes', JSON.stringify(notesList))
        },
        getNotes() {
            const localNotes = localStorage.getItem('notes')
            if (localNotes) {
                this.notes = JSON.parse(localNotes)
            }
        },
        displayNote(note) {
            const tags = Array.from(this.chosenTags) 
            const newNote = {
                note,
                tags,
                type: 'text'
            }
            this.notes.push(newNote)
            this.chosenTags = []
            this.updateNotes(this.notes)
        },
        changeNoteText(index, value) {
            this.notes[index].note = value
            this.updateNotes(this.notes)
        },
        deleteNote(index) {
            this.notes.splice(index, 1)
            this.updateNotes(this.notes)
        },
        changeNoteTag(index, value) {
            if (this.notes[index].tags.includes(value)) {
                this.notes[index].tags.splice(this.notes[index].tags.indexOf(value), 1)
            }
            else {
                this.notes[index].tags.push(value)
            }
            this.updateNotes(this.notes)
        },
        changeTag(value) {
            if (this.chosenTags.includes(value)) {
                this.chosenTags.splice(this.chosenTags.indexOf(value), 1)
            }
            else {
                this.chosenTags.push(value)
            }
        }
    }
}
</script>

<style lang="scss">
.notes-list {
    padding: 40px 0;
}
</style>
