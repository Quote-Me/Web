<template>
    <div class="home">
        <div
            class="image"
            v-if="activePhoto"
            :style="{ 'background-image': 'url(' + activePhoto + ')' }"
        ></div>
        <h2>
            {{ activeQuote.quote }}
        </h2>
        <h4>
            {{ activeQuote.author }}
        </h4>
        <button class="button" @click="refresh()">Next</button>
    </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({})
export default class Home extends Vue {
    activePhoto: string | null = null;
    activeQuote: Object | null = null;

    photos: any = [];
    quotes: any = [];

    private randomElement(min: number, max: number): number {
        return Math.floor(Math.random() * (max - min) + min);
    }

    private async setActivePhoto() {
        const photo = this.photos[this.randomElement(0, this.photos.length - 1)];
        this.activePhoto = photo.photo;
    }

    private async setActiveQuote() {
        const quote = this.quotes[this.randomElement(0, this.quotes.length - 1)];
        this.activeQuote = quote;
    }
    private async loadPhotos(){
        this.photos = await fetch(
            "https://raw.githubusercontent.com/Quote-Me/Data/main/photos.json"
        ).then((r) => r.json());
    }
    private async loadQuotes(){
        this.quotes = await fetch(
            "https://raw.githubusercontent.com/Quote-Me/Data/main/quotes.json"
        ).then((r) => r.json());
    }
    async mounted() {
        await this.loadPhotos();
        await this.loadQuotes();

        await this.setActivePhoto();
        await this.setActiveQuote();
    }
    async refresh(){
        await this.setActivePhoto();
        await this.setActiveQuote();
    }
}
</script>
<style scoped>
.image {
    background-size: contain;
    width: 100%;
    height: 450px;
    background-repeat: no-repeat;
    background-position: center center;
}
h2,
h4 {
    font-family: "DotGothic16", sans-serif;
    padding: 10px;
}
.button {
    text-align: center;
    font-weight: bold;
    letter-spacing: 3px;
    text-transform: uppercase;
    text-decoration: none;
    /*Button Color & Border*/
    border: 4px solid #3c14d0;
    color: #3c14d0;
    background: #fff;
    outline: none;
    /*Position, Display, Size*/
    position: relative;
    display: inline-block;
    padding: 15px 10px 14px;
    cursor: pointer;
    width: auto;
}
.button:after {
    /*Make a:after be as big as button*/
    position: absolute;
    width: 100%;
    height: 100%;
    /*Give a:after Border & Background color*/
    border: 2px solid #3c14d0;
    background-color: #3c14d0;
    /*Decide Location of a:after..this gives a           bottom right shadow*/
    left: 4px;
    top: 4px;
    /*Place a:after behind button*/
    z-index: -1;
    content: "";
    /*Animation/Transition Speed*/
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -o-transition: all 0.5s;
}
.button:hover {
    top: 2px;
    left: 2px;
}
.button:hover:after {
    top: -2px;
    left: -2px;
}
</style>
