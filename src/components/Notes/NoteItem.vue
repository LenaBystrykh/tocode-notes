<template>
    <div class="note-item">
        <div class="note" v-if="!isEdit">
            <div class="note-content">
                <div class="note-text">{{ text }}</div>
                <div class="note-tags">
                    <span class="preview" v-for="tag in chosenTags" :key="tag">{{ tag }}</span>
                </div>
                
                
            </div>
            <div class="btns">
                <p class="edit-btn" @click="isEdit = true">&#x270e;</p>
                <p class="delete-btn" @click="$emit('delete')">&#10005;</p>
            </div>
            
            <!-- <div class="note-footer">
                <TagsList 
                    isPreview
                    v-if="chosenTags && chosenTags.length > 0" 
                    :items="chosenTags" />
            </div> -->
        </div>
        <div class="editable-note" v-else>
            <!-- При изменении значения внутри инпута срабатывает @input и отправляет эмит с новым значением -->
            <!-- Окончанием редактирование считается нажатие на энтер, как только энтер нажат, отправляется эмит стопэдит -->
            <input 
                :value="text"
                @input="(event) => $emit('updateValue', event.target.value)" 
                @keydown.enter.prevent="isEdit = false">
            <TagsList 
                :items="allTags" 
                :chosen="chosenTags"
                @changeTag="(value) => $emit('changeTag',value)" />
        </div>
        </div>
</template>

<script>
import TagsList from '@/components/UI/TagsList'
export default {
    components: { TagsList },
    props: {
        text: {
            type: String,
            required: true
        },
        chosenTags: {
            type: Array,
            default() { 
                return [] 
            }
        },
        allTags: {
            type: Array,
            default() {
                return ['home', 'work', 'travel', 'study', 'pets', 'sport']
            }
        }
    },
    data() {
        return {
            isEdit: false
        } 
    },
    methods: {
        changeNoteTag(value) {
            console.log(value)
        }
    }
}
</script>

<style lang="scss">
.note{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.note-item {
  width: 100%;
  padding: 18px 20px;
  margin-bottom: 20px;
  border-radius: 14px;
  background-color: #ffffff;
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.02);
  &:hover {
    box-shadow: 0 30px 30px rgba(0, 0, 0, 0.04);
    transform: translate(0, -6px);
    transition-delay: 0s !important;
  }
}
.note-tags{
    margin-top: 8px;
}
.note-footer {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: start;
}
.btns {
    width: 50px;
    display: flex;
    justify-content: space-between;
}
.delete-btn{
    cursor: pointer;
    color: red;
    font-weight: bold;
}
.edit-btn{
    cursor: pointer;
    color: #494ce8;
}
.preview{
    padding-right: 16px;
    color: #444ce0;
    cursor: default;
    &:before {
        content: '#';
    }
}
</style>