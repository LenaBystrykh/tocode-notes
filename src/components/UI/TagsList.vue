<template>
    <div class="tags-list">
        <div 
            class="tag-item" 
            v-for="tag in items" 
            :key="tag" 
            @click="$emit('changeTag', tag)">
            <!-- :class="{ preview: isPreview, active: isActive }" -->
            <!-- ^ сначала название класса, затем - название пропа -->
            <div v-if="!chosen.includes(tag)" class="usualTag">
                {{ tag }}
            </div>
            <div v-else class="chosenTag">
                {{ tag }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        items: {
            type: Array,
            required: true
        },
        isPreview: {
            type: Boolean,
            default: false
        },
        isActive: {
            type: Boolean,
            default: false
        },
        chosen: {
            type: Array,
            default() { 
                return [] 
            }
        }
    }
}
</script>

<style lang="scss">
.tags-list {
    padding: 10px 0;
    display: flex;
    justify-content: center;
}
.tag-item {
    .chosenTag {
        padding: 8px 22px;
        margin-right: 10px;
        background-color: #fff;
        border-radius: 22px;
        user-select: none;
        cursor: pointer;
        background-color: #444ce0;
        color:#fff;
    }
    .usualTag {
        padding: 8px 22px;
        margin-right: 10px;
        background-color: #fff;
        border-radius: 22px;
        user-select: none;
        cursor: pointer;
        background-color: #fff;
        color: #000;
    }
    &.preview {
        padding: 0;
        color: #444ce0;
        cursor: default;
        &:before {
            content: '#';
        }
    }
    &:last-child {
        margin-right: 0;
    }
}
</style>
