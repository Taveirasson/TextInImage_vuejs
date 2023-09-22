<template>
    <div class="form-group">
        <div id="meme-gallery" class="border"> 
            <span v-for="(image, index) in gallery" :key="index">
              <img :src="image" class="image-gallery" @click="emitUpdateImage(image)">
            </span>
          </div>
        </div>
</template>
<script>

export default {
    name: "ImageGallery",
    data(){
        return{
            gallery: [
                "images/I-Have-No-Idea-What-I-Am-Doing-Dog.jpg", 
                'images/owl.jpg',
                'images/lion.avif',
                'images/catdogs.jpg',
                'images/comedy-wildlife-awards-squirel-stop.jpg',
                'images/dogglass.jpg',
                'images/seal.jpg',
                'images/cats.jpg',
                'images/zebra.jpg',
                
                ],
        }
    },
    methods:{
        emitUpdateImage(image){
            this.$emit('emit-update-image', image);
        },
    },
}
</script>
<style>
    #meme-gallery {
        word-wrap: break-word;
        height: 100px;
        width: 100%;
        overflow: scroll;
        overflow-y: hidden;
        white-space: nowrap;
    }

    .image-gallery {
        width: 90px;
        height: 100%;
        cursor: pointer;
    }
</style>