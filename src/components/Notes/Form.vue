<template>
    <div class="notes-wrapper">
        <form class="note-form" @submit.prevent="sendNote">
            <textarea 
                required 
                v-model="inputValue" 
                placeholder="Type your note" />
            <TagsList @chooseTag="getTag" :items="tags" />
            <button class="btn btnPrimary" type="submit">Add note</button>
        </form>
    </div>
</template>

<script>
import TagsList from '@/components/UI/TagsList'
export default {
    components: {TagsList},
    data() {
        return {
            inputValue: '',
            tags: ['home', 'work', 'travel', 'study', 'pets', 'sport']
        }
    },
    methods: {
        sendNote() {
            this.$emit('sendNote', this.inputValue)
            this.inputValue = ''
        },
        getTag(tag) {
            console.log(tag)
        }
    }
}
</script>

<style lang="scss">
.notes-wrapper{
    display: flex;
    flex-direction: column;
    width: 70%;
    margin: auto;
    min-width: 300px;
}
.note-form {
    display: flex;
    flex-direction: column;
    width: 100%;
    textarea {
        margin-bottom: 0;
    }
}
</style>