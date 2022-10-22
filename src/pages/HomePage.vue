<template>
    <Form @sendNote="displayNote" :tags="tags" :chosen="chosenTags" @changeTag="(value) => changeTag(value)"/>
    <div class="notes-list">
        <!-- Такой вариант ошибочный, так как на первом месте должен обязательно идти сам элемент -->
        <!-- <div class="note-item" v-for="(index, note) in items" :key="index"> -->
        <!-- Корректным является вариант ниже -->
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
    watch: {
        notes: {
            handler(updatedList) {
                localStorage.setItem('notes', JSON.stringify(updatedList))
            },
            deep: true
        }
    },
    methods: {
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
        },
        changeNoteText(index, value) {
            this.notes[index].note = value
        },
        deleteNote(index) {
            this.notes.splice(index, 1)
        },
        changeNoteTag(index, value) {
            if (this.notes[index].tags.includes(value)) {
                this.notes[index].tags.splice(this.notes[index].tags.indexOf(value), 1)
            }
            else {
                this.notes[index].tags.push(value)
            }
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