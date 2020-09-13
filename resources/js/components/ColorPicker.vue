<template>
    <div id=color-picker>
        <h1>Terrain picker</h1>
        <div id="palet">
            <div v-for="(terrain, index) in terrains" 
                :key="index" class="item" v-bind:class="[terrain]" 
                v-on:click="setTerrain(terrain)"></div>
        </div>
        <div class="item selected" v-bind:class="[selectedTerrain]"></div>
    </div>
</template>

<script>
export default {
    name: "terrain-picker",
    mounted() {
        this.setTerrain(this.terrains[0]);
    },
    data() {
        return {
            terrains: [
                "grass",
                "stone",
                "path",
                "trees",
                "water",
                "snow"
            ],
            selectedTerrain: ""
        }
    },
    methods: {
        setTerrain(terrain) {
            window.selectedTerrain = terrain;
            this.selectedTerrain = terrain;
        }
    }
}
</script>

<style scoped>
    #color-picker {
        margin:auto;
        position: relative;
    }

    #palet {
        max-width: 15rem;
        
        display: grid;
        grid-template-columns: repeat(5, auto);
        
    }

    .item {
        padding-top: 100%;
        cursor: pointer;
    }

    .item.selected {
        position: absolute;
        width: 3rem;
        height: 3rem;
        left: -5rem;
        top: 1.7rem;
        padding-top: initial;
    }

    .item.grass{ background-color: greenyellow; }
    .item.stone{ background-color: gray; }
    .item.path{ background-color: burlywood; }
    .item.trees{ background-color: green; }
    .item.water{ background-color: aqua; }
    .item.snow{ background-color: whitesmoke; }
</style>