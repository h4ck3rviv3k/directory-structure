<template>
    <div id="app">
        <ul>
            <tree-item :parentData="parentData" :key="-1" :depth="0" class="item" @save-data="saveData">
            </tree-item>
        </ul>
    </div>
</template>

<script>
import TreeItem from "./components/TreeItem.vue";

var treeData = {
    id:1,
    name: "Directory",
    children: [],
};

const storageKey = "directoryKey";

export default {
    name: "App",
    components: {
        TreeItem: TreeItem,
    },
    created() {
        let storedObj = localStorage.getItem(storageKey);
        if (storedObj !== null) {
            this.parentData = JSON.parse(storedObj);
            console.log(this.parentData);
        }else{
            this.parentData = treeData;
        }
    },
    data: function () {
        return {
            parentData: undefined,
        };
    },
    methods: {
        saveData: function (item) {
            console.log(item)
            this.parentData=JSON.parse(JSON.stringify(item));
            localStorage.setItem(storageKey, JSON.stringify(this.parentData));
            console.log(this.$data.parentData);
        }
    },
};
</script>

<style>
</style>
