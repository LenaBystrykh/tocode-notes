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
            let allTags = document.getElementsByClassName('tag-item')
            let chosenTags = []
            for (let tag of allTags) {
                if (tag.classList.contains('active')) {
                    chosenTags.push(tag.innerText)
                }
            }
            this.$emit('sendNote', {'note': this.inputValue, 'tags': chosenTags})
            this.inputValue = ''
            for (let tag of allTags) {
                if (tag.classList.contains('active')) {
                    tag.classList.remove('active')
                }
            }
        },
        getTag(tag) {
            event.target.classList.toggle("active")
            console.log(event.target.classList)
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