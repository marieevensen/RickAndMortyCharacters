<template>
    <main class="rick-and-morty" v-if="!error">
        <img class="rick-and-morty__header" src="/images/rickandmorty.png" alt="Rick and Morty logo">

        <article class="rick-and-morty__character">
            <img class="character__image" :src="activeCharacter.image" alt="Picture of the character">

            <section class="character__info">
                <h1 class="info__name">{{ activeCharacter.name }}</h1>

                <dl>
                    <dt>Status: {{ activeCharacter.status }}</dt>

                    <dt>Species: {{ activeCharacter.species }}</dt>
                    
                    <dt>Gender: {{ activeCharacter.gender }}</dt>

                    <dt v-if="activeCharacter.type.length > 0">
                        Type: {{ activeCharacter.type }}
                    </dt>
                    
                    <dt>Last known location: {{ activeCharacter.location.name }}</dt>
                    
                    <dt>Origin: {{ activeCharacter.origin.name }}</dt>
                </dl>
            </section>
        </article>
    </main>

    <div v-if="error">
        {{ error }}
    </div>
</template>

<script>
    export default {
        data () {
            return {
                characters: [],
                activeCharacter: {},
                error: ''
            }
        },

        created() {
            this.fetchValue();
        },

        methods: {
            async fetchValue() {
                const url = `https://rickandmortyapi.com/api/character`;
                const respond = await fetch(url);
                try {
                    await this.handleRespond(respond);
                } catch (error) {
                    this.error = error.message;
                }
            },
             
			async handleRespond(respond) {
				if(respond.status >= 200 && respond.status < 300) {
                    const result = await respond.json();
                    this.characters = result.results;
                     
                    this.setCharacterActive()

                    return true;
					
				} else {
					if(respond.status === 404) {
						throw new Error('Url ikke funnet!');
					}
					if(respond.status === 401) {
						throw new Error('Ikke authorisert!');
					}
					if(respond.status > 500) {
						throw new Error('Server error!');
					}
					throw new Error('Noe gikk galt!');
				}
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
        box-shadow: 0px 0px 10px 5px var(--details);
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

    dt {
        padding-top: 10px;
    }
</style>