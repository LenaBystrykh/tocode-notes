<template>
    <div class="notes-list">
        <!-- Такой вариант ошибочный, так как на первом месте должен обязательно идти сам элемент -->
        <!-- <div class="note-item" v-for="(index, note) in items" :key="index"> -->
        <!-- Корректным является вариант ниже -->
        <div class="note-item" v-for="(note, index) in items" :key="index">
            <!-- <div v-if="items[index].type=='text'" class="note-header"> -->
            <div class="note-header">
                <p>{{ note.note }}</p>
                <div class="btns">
                    <!-- Вот как мы бы это сделали раньше: -->
                    <!-- <p class="edit-btn" @click="edit(index)">&#x270e;</p> -->
                    <!-- <p class="remove-btn" @click="remove(index)">&#10005;</p> -->
                    <!-- А вот так с эмитами: -->
                    <!-- <p class="edit-btn" @click="$emit('edit', index)">&#x270e;</p> -->
                    <p class="remove-btn" @click="$emit('remove', index)">&#10005;</p>
                </div>
            </div>
            <!-- <input v-else-if="items[index].type == 'edit'" v-model="items[index].note" @keydown="$emit('stopEdit', index)"> -->
            <div class="note-footer">
                <TagsList 
                    isPreview
                    v-if="note.tags && note.tags.length > 0" 
                    :items="note.tags" />
            </div> 
        </div>
    </div>
</template>

<script>
import TagsList from '@/components/UI/TagsList'
export default {
    components: {TagsList},
    props: {
        items: {
            type: Array,
            required: true
        }
    }
}
</script>

<style lang="scss">
.notes-list {
  padding: 40px 0;
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
.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
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
.remove-btn{
    cursor: pointer;
    color: red;
    font-weight: bold;
}
.edit-btn{
    cursor: pointer;
    color: #494ce8;
}
</style>