<template>
    <Form @sendNote="displayNote"/>
    <!-- <List @edit="edit" @remove="remove" :items="notes"/> -->
    <List @remove="remove" @edit="edit" @stopEdit="stopEdit" :items="notes"/>
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
                    tags: ['work', 'home']
                },
                {
                    note: 'task 2',
                    tags: ['pets']
                },
                {
                    note: 'task 3',
                    tags: []
                }]
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
                tags: noteTags
            }
            this.notes.push(note)
        },
        
        remove(index) {
            this.notes.splice(index, 1)
        },
        // edit(index) {
        //     this.notes[index].type = 'edit';
        // },
        // stopEdit(el, index) {
        //     if(event.key === 'Enter') {
        //         this.notes[index].type = 'text';
        //     }
        // }
    }
}
</script>