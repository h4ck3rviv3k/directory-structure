<template>
    <li>
        <div @click="toggle" @dblclick="makeFolder">
            {{ childData.name }}
            <span v-if="isFolder">[{{ isOpen ? "-" : "+" }}]</span>
        </div>
        <ul v-if="isFolder" v-show="isOpen">
            <tree-item
                    v-for="(child, index) in childData.children"
                    :key="index"
                    :depth="depth+1"
                    :parentData="child"
                    class="item"
                    @save-data="saveData"
            />
            <li @click="addItem">+</li>
        </ul>
    </li>
</template>

<script>

export default {
    name: "TreeItem",
    props: {
        parentData: Object,
        depth: Number
    },
    created() {
        this.childData = JSON.parse(JSON.stringify(this.parentData))
    },
    data: function () {
        return {
            isOpen: false,
            childData: undefined,
            count: 1
        };
    },
    computed: {
        isFolder: function () {
            return this.childData.children && this.childData.children.length;
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
                this.addItem();
                this.isOpen = true;
            }
        },
        addItem: function () {
            let id = this.parentData.id.toString() + this.increaseCount();
            this.childData.children.push({id: id, name: "File", children: []});
            this.childData = JSON.parse(JSON.stringify(this.childData));
            this.$emit("save-data", this.childData);
        },
        saveData: function (item) {
            let shouldAdd = true;
            this.childData.children.forEach((obj, index) => {
                if (obj.id === item.id) {
                    this.childData.children[index] = item;
                    shouldAdd = false;
                }
            });
            if (shouldAdd) {
                this.childData.children.push(item);
            }
            this.$emit("save-data", this.childData);
            console.log("childData ==>" + this.childData.toString());
        },
        increaseCount: function () {
            return this.count++;
        }
    },
};
</script>

<style scoped>
</style>