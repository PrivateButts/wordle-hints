<template>
    <div>
        <div v-for="l in letters" :key="l" class="letter" :class="{'selected': selected.includes(l)}" v-on:click="toggleSelected(l)">
            {{ l }}
            <div v-if="selected.includes(l)" class="selected-overlay">
                <div class="letter" :class="[l in output && output[l].includes(i) ? selectedClass : '']" v-for="i in 5" :key="i" v-on:click.prevent.stop="togglePos(l,i)">
                    {{ i }}
                </div>
                <div class="letter" :class="{'selected': l in output && output[l].length == 5}" v-on:click.prevent.stop="toggleAllPos(l)">
                    Any
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: "LetterSelect",
    props: ['value', 'selectedClass'],
    data () {
        return {
            letters: [],
            selected: [],
            output: {},
        }
    },
    methods: {
        toggleSelected (letter) {
            if (this.selected.includes(letter)) {
                this.selected = this.selected.filter(l => l !== letter);
            } else {
                this.selected.push(letter);
            }
        },
        togglePos (letter, pos) {
            if (!(letter in this.output)) {
                this.$set(this.output, letter, []);
            }

            if(this.output[letter].includes(pos)) {
                this.output[letter] = this.output[letter].filter(p => p !== pos);
                if(this.output[letter].length === 0) {
                    delete this.output[letter];
                }
            } else {
                this.output[letter].push(pos);
            }
        },
        toggleAllPos(letter){
            for(var i=1; i<6; i++){
                console.log(i)
                this.togglePos(letter, i);
            }
        }
    },
    watch: {
        output: {
            handler (newVal) {
                this.$emit('input', newVal);
            },
            deep: true
        }
    },
    mounted () {
        // this.selected = this.value;

        const alpha = Array.from(Array(26)).map((e, i) => i + 97);
        this.letters = alpha.map((x) => String.fromCharCode(x));
    },
};
</script>

<style scoped>
.letter {
    display: inline-block;
    padding: 10px;
    margin: 3px;
    border: 1px solid black;
    border-radius: 5px;
    cursor: pointer;
    text-transform: uppercase;
    background-color:#fff;
}
.letter.selected {
    background-color: #ccc;
}
.letter.selected-red {
    background-color: #f00;
}
.letter.selected-green {
    background-color: #0f0;
}
</style>
