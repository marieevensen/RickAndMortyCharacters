<template>
    <main class="rick-and-morty">
        <img class="rick-and-morty__header" src="/images/rickandmorty.png" alt="Rick and Morty">

        <div class="rick-and-morty__character">
            <img class="character__image" :src="activeCharacter.image" alt="">

            <div class="character__info">
                <div class="info__name">        
                    {{ activeCharacter.name }}
                </div>

                <h4>Status: {{ activeCharacter.status }}</h4>

                <h4>Species: {{ activeCharacter.species }}</h4>
           
                <h4>Gender: {{ activeCharacter.gender }}</h4>

                <div v-if="activeCharacter.type.length > 0">
                    <h4>Type: {{ activeCharacter.type }}</h4>
                </div>
      
                <h4>Last known location: {{ activeCharacter.location.name }}</h4>
      
                <h4>Origin: {{ activeCharacter.origin.name }}</h4>
            </div>
        </div>
    </main>
</template>

<script>
    export default {
        data () {
            return {
                characters: [],
                activeCharacter: {},
            }
        },

        created() {
            this.fetchValue();
        },

        methods: {
            async fetchValue() {
                const url = `https://rickandmortyapi.com/api/character`;
                const respond = await fetch(url);
                const { results } = await respond.json();
                
                this.characters = results;

                this.setCharacterActive()
            },

            setCharacterActive () {
                const random = Math.floor(Math.random() * this.characters.length);
                this.activeCharacter = this.characters[(random - 1)];
            }
        }
    }
</script>

<style>
    .rick-and-morty {
        text-align: center;
    }

    .rick-and-morty__header {
        width: 45vw;
    }

    @media screen and (min-width: 1000px) {
        .rick-and-morty__character {
            display: flex;
        }
    }

    .character__image {
        height: 35vh;
        width: auto;
        margin-top: 20px;
        box-shadow: 0px 0px 10px 5px #85FF48;
    }

    @media screen and (min-width: 1000px) {
        .character__image {
            height: 50vh;
            margin: 50px 0px 0px 150px;
        }
    }    

    .character__info {
        padding: 30px;
        line-height: 30px;
    }

    @media screen and (min-width: 1000px) {
        .character__info {
            text-align: left;
            margin: 20px 0px 0px 90px;
            padding-right: 30px;
            line-height: 40px;
        }
    } 

    .info__name {
        font-size: 45px;
        font-weight: bold;
        line-height: 60px;
    }
</style>