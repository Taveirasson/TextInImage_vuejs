<template>
 <div id="app" class="row m-0 py-2 text-center">

      <div id="main-content" class="col-md-6 d-block mx-auto position relative">
            <p class="fixed-top text" :style="textStyle">{{text.top}}</p>
            <img id="main-image" class="image" :src="image.src" :title="image.title" :alt="Imagem">
            <p class="fixed-bottom text" :style="textStyle">{{text.bottom}}</p>
      </div>

      

      <div class="col-md-6">
        <h1 class="mt-2">
           Meme Text
        </h1>
        <div class="form-group">
          <input type="url" placeholder="Link Imagem" class="form-control" v-model="image.src"> 
        </div>

        <div class="form-group">
          <input type="text" placeholder="Top text" class="form-control" v-model="text.top" :maxlength="text.maxCarac">
          <br>
          <input type="text" placeholder="Bottom text" class="form-control" v-model="text.bottom" :maxlength="text.maxCarac">
        </div>
        
        <div class="row m-0 py-2 text-center">
            <div class="col-md-6 d-block mx-auto position relative">
                <textColor  @update-shadow-color="updateShadowColor" @update-text-color="updateTextColor" :colors="colors" />
            </div>
    
            <div class="text-style col-md-6">
                <label>Font Size</label><br>
                <input class="wide-input" type="number" placeholder="Text Size" v-model="text.size" @input="updateTextSize" :min="text.minSize" :max="text.maxSize">
                 <br><br>
                <label>Text Shadow</label><br>
                 <input type="checkbox" class="checkboxInput" :checked="text.withshadow" @click="toggleTextShadow()">
                 <br>
                 <label>Text Uppercase</label><br>
                 <input type="checkbox" class="checkboxInput" :checked="text.upper"  @click="toggleTextUpper()"> 
            </div>
             <select class="form-control" v-model="text.fontfamily">
             <option v-for="(fontfamily, index) in fontfamilys" :key="index" @input="uptadeFontFamily()">
              {{fontfamily}}
             </option>
           </select>
        </div>
        <div class="buttons">
            <div class="form-group">
            <button class="btn btn-outline-primary" @click="resetInputs()">Reset</button>
            </div>
            <div class="form-group">
            <button class="btn btn-outline-danger" @click="dowloadImageText()">Dowload Image</button>
            </div>
        </div>
      </div>
     <div class="col-md-6">
            <br>
            <imageGallery @emit-update-image='updateImage' />
     </div>

    </div>
</template>

<script>
var srcFirstImage = "images/lion.avif";

import html2canvas from 'html2canvas';

import TextColor from './textColor.vue';
import ImageGallery from './ImageGallery.vue';

export default {

    name: "ImagemEdit",
    components:{
        TextColor,
        ImageGallery
    },
    data(){
        return{
            image:{
                src: "",
                title: "Imagem Principal",
            },
            text:{
                top: '',
                bottom: '',
                color: '',
                withshadow: false,
                shadow: '',
                upper: false,
                size: '',
                minSize: '20',
                maxSize: '130',
                maxCarac: '81',
                fontfamily: 'Passion One',
            },
            colors: ['black','white','red','blue','green','yellow'],
            fontfamilys: ['Passion One','Arial', 'Ubuntu'],
        };
    },
    created(){
        this.image.src = srcFirstImage;
    },
    computed:{
        textStyle(){
          let textShadow  = this.text.withshadow ? '3px '+'3px '+'3px '+this.text.shadow : 'none';
          let textUpper = this.text.upper ? 'uppercase': 'lowercase';
          
          return{
                color: this.text.color,
                fontSize: this.text.size+"px",
                textShadow: textShadow ,
                textTransform: textUpper,
                fontFamily: this.text.fontfamily,
            };
        },
    },
    methods: {
        resetInputs() {
            this.text.top = "";
            this.text.bottom = "";
            this.text.color = "";
            this.text.size = "100";
        },
        
        updateTextColor(color){
            this.text.color = color;
        },
        updateTextSize(){
            this.text.size = event.target.value;
        },
        updateShadowColor(color){
            this.text.shadow = color;
        },

        toggleTextShadow() {
            this.text.withshadow = !this.text.withshadow;
        },
        toggleTextUpper(){
            this.text.upper = !this.text.upper;
        },

        uptadeFontFamily(){
            this.text.fontfamily = event.target.value;
        },

        updateImage(image){
            this.image.src = image;
        },

        async dowloadImageText(){
            const elementToCapture = document.getElementById('main-content');

            try{
                const canvas = await html2canvas(elementToCapture);

                const imageBlob = await new Promise((resolve)=>{
                    canvas.toBlob(resolve,'image/png')
                });

                const imageUrl = URL.createObjectURL(imageBlob);

                const a = document.createElement('a');
                a.href = imageUrl;
                a.download = 'image_with_text.png';

                a.click();
                URL.revokeObjectURL(imageUrl);
            }
            catch (error) {
                console.error('Error while saving the image', error);
            }
        }
    },
};
</script>
<style scoped>

.wide-input {
  width: 200px; 
}

.checkboxInput{
    width: 20px;
    height: 20px;
}

</style>>