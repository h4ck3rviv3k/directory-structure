<template>
    <li>
        <div @click="toggle"
             @dblclick="makeFolder">
            {{ item.name }}
            <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
        </div>
        <ul v-if="isFolder" v-show="isOpen">
            <tree-item
                    v-for="(child, index) in item.children"
                    :key="index"
                    :item="child"
                    class="item"
                    @make-folder="$emit('make-folder', $event)"
                    @add-item="$emit('add-item', $event)"/>
            <li @click="$emit('add-item', item)">+</li>
        </ul>
    </li>
</template>

<script>
export default {
    name: "TreeItem",
    props: {
        item: Object
    },
    data: function () {
        return {
            isOpen: false
        };
    },
    computed: {
        isFolder: function () {
            return this.item.children && this.item.children.length;
        }
    },
    methods: {
        toggle: function () {
            if (this.isFolder) {
                this.isOpen = !this.isOpen;
            }
        },
        makeFolder: function () {
            if (!this.isFolder) {
                this.$emit("make-folder", this.item);
                this.isOpen = true;
            }
        }
    }
}
</script>

<style scoped>

</style>