<template>
    <main class="rick-and-morty">
        <img class="rick-and-morty__header" src="/images/rickandmorty.png" alt="Rick and Morty logo">

        <article class="rick-and-morty__character">
            <img class="character__image" :src="activeCharacter.image" alt="Picture of the character">

            <section class="character__info">
                <p class="info__name">{{ activeCharacter.name }}</p>

                <dl>
                    <dt>Status: {{ activeCharacter.status }}</dt>

                    <dt>Species: {{ activeCharacter.species }}</dt>
                    
                    <dt>Gender: {{ activeCharacter.gender }}</dt>
                    
                    <dt v-if="activeCharacter.type.length > 0">
                        Type: {{ activeCharacter.type }}</dt>
                    
                    <dt>Last known location: {{ activeCharacter.location.name }}</dt>
                    
                    <dt>Origin: {{ activeCharacter.origin.name }}</dt>
                </dl>
            </section>
        </article>
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

<!-- Kommenterer script
    1 Henter ut info fra api
    2 Setter resultatet inn i en array kalt characters
    3 Setter karakteren aktiv, det er den som vises
    4 Plukker ut en random karakter ut av array, hver gang man refresher siden
-->

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
        height: 40vw;
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
        font-weight: bold;
        line-height: 40px;
    }
</style>