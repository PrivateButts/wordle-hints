<template>
    <div id="app" class="container mt-3">
        <h1>Wordle Hints</h1>
        <div class="row">
            <div class="col-lg">
                <label>Configure Exclusions</label>
                <letter-select v-model="filterExclude" selected-class="selected-red"></letter-select>

                <label class="mt-2">Configure locked in letters</label>
                <letter-select v-model="filterInclude" selected-class="selected-green"></letter-select>
            </div>
            <div class="col-lg">
                <h3 class="mt-3">{{ solutions.length }} Possible Solutions</h3>
                <ul>
                    <li v-for="s in solutions" :key="s">{{s}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import Solutions from './data/solutions.json';
import LetterSelect from "./components/LetterSelect.vue";

export default {
    name: "App",
    components: {
        LetterSelect,
    },
    data () {
        return {
            filterExclude: {},
            filterInclude: {},
        }
    },
    computed: {
        solutions () {
            return Solutions.filter(w => {
                // filter to words containing locked in letters and letters that are in the exclude list but not excluded from every position
                let filterInclude = this.filterInclude;
                for(let l in filterInclude){
                    for(let p of filterInclude[l]){
                        if(w[p-1] !== l){
                            return false;
                        }
                    }
                }

                // filter to words not containing excluded letters
                let filterExclude = this.filterExclude;
                for(let l in filterExclude) {
                    if(filterExclude[l].length !== 5) {
                        if(!w.includes(l)) {
                            return false;
                        }
                    }
                    for(let p of filterExclude[l]) {
                        if(w[p-1] == l) {
                            return false;
                        }
                    }
                }
                return true;
            });
        },
    }
};
</script>
