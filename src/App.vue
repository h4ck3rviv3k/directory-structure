<template>
    <div id="app">
        <ul>
            <tree-item
                    :item="treeData"
                    class="item"
                    @make-folder="makeFolder"
                    @add-item="addItem">
            </tree-item>
        </ul>
    </div>
</template>

<script>
import TreeItem from './components/TreeItem.vue'
import Vue from 'vue'

var treeData = {
    name: "Directory",
    children: []
};

const storageKey = "directoryKey";

export default {
    name: 'App',
    components: {
        TreeItem: TreeItem
    },
    mounted() {
        let storedObj = localStorage.getItem(storageKey);
        if (typeof storedObj !== 'undefined' || storedObj !== null) {
            treeData = JSON.parse(storedObj);
        }
        console.log(treeData)
    },
    data: function () {
        return {
            treeData: treeData
        }
    },
    methods: {
        makeFolder: function (item) {
            Vue.set(item, "children", []);
            this.addItem(item);
        },
        addItem: function (item) {
            item.children.push({
                name: "File"
            });
            localStorage.setItem(storageKey, JSON.stringify(item));
            console.log(item)
        }
    }
}
</script>

<style>

</style>
