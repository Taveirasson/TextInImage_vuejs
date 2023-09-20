<template>
    <div>
        <div class="form-group">
            <label>Text Color</label><br>
            <span v-for="(color,index) in colors" :key="index" >
                <span id = "span-color" :style="{background: color}" @click="emitUpdateTextColor(color)">
                </span>
            </span>
        </div>
        <div class="form-group">
            <label>Text Shadow Color</label><br>
            <span v-for="(color,index) in colors" :key="index" >
                <span id = "span-color" :style="{background: color}" @click="emitUpdateShadowColor(color)">
                </span>
            </span>
            
        </div>
    </div>
</template>
<script>

export default {
    name: "TextColor",
    props: {
        colors:{
            type: Array,
            required: true,
        }
    },
    methods:{
        emitUpdateTextColor(color){
            this.$emit('update-text-color', color);
        },
        
        emitUpdateShadowColor(color){
            this.$emit('update-shadow-color', color);
        },
    },
}
</script>
<style>

  #span-color {
    border: 1px solid #000;
    border-radius: 50%;
    cursor: pointer;
    display: inline-block;
    width: 20px;
    height: 20px;
    margin-left: 5px;
  }

</style>