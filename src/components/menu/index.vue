<template>
    <div class="vui-menu">
        <menu-items :menuData="selfMenuData" :showOne="showOne" :mapTable="mapTable" @click="click"></menu-items>
    </div>
</template>

<script>
    import MenuItems from "./items.vue";
    export default {
        name: "vMenu",
        data() {
            return {
                selfMenuData: [],
                slefMapTable: { ...this.mapTable }
            };
        },
        props: {
            menuData: Array,
            showOne: Boolean,
            mapTable: Object
        },
        components: {
            MenuItems
        },
        created() {
            const items = !this.menuData || this.menuData.length === 0 ? [] : [...this.menuData];
            function loopData(data, deep) {
                data.forEach(item => {
                    item._deep = !deep ? 0 : deep;
                    item._showChildren = false;
                    item._actived = false;
                    if (item.children && item.children.length > 0) {
                        item._hasChildren = true;
                        item.children = [...item.children];
                        loopData(item.children, item._deep + 1);
                    } else {
                        item._hasChildren = false;
                    }
                });
            };
            loopData(items);
            this.selfMenuData = items;
        },
        methods: {
            click(items) {
                this.$emit('click', { ...items });
            }
        }
    };
</script>

<style scoped>
</style>
