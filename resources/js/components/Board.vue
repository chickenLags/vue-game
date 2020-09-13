<template>
    <div id="board">
        <h1>board</h1>
        <div id="grid">
            <div v-for="(tile, index) in tiles" :key="index" 
                class="tile" v-bind:class="[tile.class]"
                v-on:mousedown="setMouseDown(index)"
                v-on:mouseenter="setTerrain(index)"
                v-on:mouseup="setMouseUp()"
                ></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "board",
    beforeMount() { 
        console.log('mounted board'); 
        // generate biased map
        for(let i = 0; i < 30 * 30; i++) {
            this.tiles.push({ class: this.getRandomClass()});
        }

        // generate road based on greedy search
        // but first convert board to searchable array
        let i,j,temparray,chunk = 30;
        let gridArray = [];
        for (i=0, j=this.tiles.length; i<j; i += chunk) {
            temparray = this.tiles.slice(i , i+chunk);
            let temperArray = temparray.map(el => {
                switch(el.class) {
                    case "grass":
                        return 1;
                    case "trees":
                        return 3;
                    case "water":
                        return 4;
                    case "stone":
                        return 7;
                    case "snow":
                        return 2;
                    default:
                        return 0;
                }
            });
            gridArray.push(temperArray);
        }
        
        let graph = new astar.Graph(gridArray);

        // start and endpoints:
        let start = graph.grid[0][10];
	    let end = graph.grid[29][20];

        // search and resulting steps in list
        let result = astar.astar.search(graph, start, end);

        // draw path on map
        result.forEach(step => {
            let index = step.x + (step.y * 30) -1;
            this.tiles[index].class = "path";
        });

        this.tiles[end.x + (end.y * 30) ].class = "path"; // start node is not included in results
        
        console.log(result);



    },
    data: function() {
        return {
            tiles: [],
            mouseDown: false,
            terrains: [ "grass", "trees", "water", "stone", "path", "snow"],
        }
    },
    methods: {
        setTerrain(index) {
            if (this.mouseDown) {
                this.tiles[index].class = window.selectedTerrain;
            }
        },
        setMouseDown(index) {
            this.mouseDown = true;
            this.setTerrain(index);
        },
        setMouseUp() {
            this.mouseDown = false;
            console.log(`set mouse up to: ${this.mouseDown}`)
        },
        getRandomClass() {
            
            let x = Math.random() * this.terrains.length -1;
            let k = Math.pow(1 - 0.3, 3);
            let input = (x * k) / (x * k  - x  + 1);
            let index = Math.ceil(input);
            
            if (index < 0 )
                index = 2;

            if (index > this.terrains.length -1)
                index = 3;

            let terrain = this.terrains[index];
            return terrain;
        }

    }
    
}
</script>

<style scoped>

    #board{
        position: relative;
    }

    #grid {
        display:grid;
        grid-template-columns: repeat(30, auto);
    }

    .tile {
        display: inline-block;
        padding-top: 100%;
    }

    .tile.grass { background-color: greenyellow; }
    .tile.path { background-color: burlywood; }
    .tile.stone { background-color: gray; }
    .tile.trees { background-color: green; }
    .tile.water{ background-color: aqua; }
    .tile.snow{ background-color: whitesmoke; }
</style>
