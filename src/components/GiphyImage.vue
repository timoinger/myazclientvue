<template>
    <div>
        <img :src="giphyUrl" :alt="name" width="200" />
    </div>
</template>

<script>
    export default {
        name: "GiphyImage",
        props: {
            name: String
        },
        data() {
            return {
                giphyUrl: String
            }
        },
        mounted() {
            this.loadImage()
        },
        methods: {
            async loadImage() {
                const giphyApi = '//api.giphy.com/v1/gifs/search?api_key=dc6zaTOxFJmzC&limit=1&q=';
                fetch(giphyApi + this.$props.name)
                .then(response => response.json())
                .then(response => {
                    if (response.data && response.data.length > 0) {
                        this.giphyUrl = response.data[0].images.original.url;
                    } else {
                        this.giphyUrl = '//media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif';
                    }
                });
            }
        }
    }
</script>

<style scoped>

</style>
