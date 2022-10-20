<template>
    <Form @sendNote="displayNote" :tags="tags"/>
    <List @remove="remove" @edit="edit" @stopEdit="stopEdit" :items="notes" :tags="tags"/>
</template>

<script>
import Form from '@/components/Notes/Form'
import List from '@/components/Notes/List'
export default {
    components: {Form, List},
    data() {
        return {
            // notes: [{note: 'task 1', type: 'text'},
            //         {note: 'task 2', type: 'text'},
            //         {note: 'task 3', type: 'text'}]
            notes: [
                {
                    note: 'task 1',
                    tags: ['work', 'home'],
                    type: 'text'
                },
                {
                    note: 'task 2',
                    tags: ['pets'],
                    type: 'text'
                },
                {
                    note: 'task 3',
                    tags: [],
                    type: 'text'
                }],
            tags: ['home', 'work', 'travel', 'study', 'pets', 'sport']
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
        displayNote({'note': noteTitle, 'tags': noteTags}) {
            const note = {
                note: noteTitle,
                tags: noteTags,
                type: 'text'
            }
            this.notes.push(note)
        },
        
        remove(index) {
            this.notes.splice(index, 1)
        },
        edit(index) {
            this.notes[index].type = 'edit'
        },
        stopEdit(index) {
            if(event.key === 'Enter') {
                let allTags = document.getElementsByClassName('tag-item')
                let chosenTags = []
                for (let tag of allTags) {
                    if (tag.classList.contains('active')) {
                        chosenTags.push(tag.innerText)
                    }
                }
                this.notes[index].tags = chosenTags
                this.notes[index].type = 'text'
            }
        }
    }
}
</script>