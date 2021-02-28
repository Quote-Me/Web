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
            {{activeQuote.author}}
        </h4>
    </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({})
export default class Home extends Vue {
    activePhoto: string | null = null;
    activeQuote: Object | null = null;

    private randomElement(min: number, max: number): number {
        return Math.floor(Math.random() * (max - min) + min);
    }

    private async setActivePhoto() {
        const photos = await fetch(
            "https://raw.githubusercontent.com/Quote-Me/Data/main/photos.json"
        ).then((r) => r.json());
        const photo = photos[this.randomElement(0, photos.length - 1)];
        this.activePhoto = photo.photo;
    }

    private async setActiveQuote(){
        const quotes = await fetch("https://raw.githubusercontent.com/Quote-Me/Data/main/quotes.json").then((r) => r.json());
        const quote = quotes[this.randomElement(0, quotes.length - 1)];
        this.activeQuote = quote;
    }
    async mounted() {
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
h2, h4{
    font-family: 'DotGothic16', sans-serif;
}
</style>
