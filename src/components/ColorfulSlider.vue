<template>
    <div class="ColorfullGalleryWrapper">
        <transition  name="fade">
            <div :class="{invert: !light}" v-if="wheelLoading" class="wheel_loading">
                <div class="wheel_loading_item_1 wheel_loading_item"></div>
                <div class="wheel_loading_item_2 wheel_loading_item"></div>
                <div class="wheel_loading_item_3 wheel_loading_item"></div>
            </div>
        </transition>
        <div v-if="sidebar"  class="sidebar">
            <div class="sidebar_item_wrapper"
                    v-for="(item, index) in captions" 
                    :key="index" 
                    @click="displace=index">
                <div class="sidebar_item">
                    <div class="sidebar_caption_container">
                        <div class="sidebar_caption" :class="{current_caption: index===displace}">{{item}}
                            <div class="sidebar_caption_line"></div>
                        </div>
                        <div class="sidebar_breakpoint"></div>
                    </div>
                </div>
            </div>
        </div>
        <div tabindex="0" class="slide_wrapper" ref="gallery">
            <div class="brush" 
                v-for="(item, index) in Nodes" 
                :key="index"
                ref="brushes">
                <img :src="require(`../assets/images/brushes/${index%7+1 }.webp`)" alt="">
            </div>
            <slot/>
        </div>
        <div :class="{invert: light}" v-if="dots" class="dots">
            <div v-if="arrows" @click="displace--" class="arr arr_right">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 972 1858">
                    <defs/>
                    <path id="Фигура_1" data-name="Фигура 1" class="cls-1" d="M42.819,1857.19l-42.43-42.43L928.82,886.326l42.431,42.431Z"/>
                    <path id="Фигура_1_копия" data-name="Фигура 1 копия" class="cls-1" d="M-0.364,42L42.067-.435,970.5,928l-42.431,42.43Z"/>
                </svg>
            </div>
            <div class="dot" 
                v-for="(item, index) in this.Nodes" 
                :class="{dotActive: displace===index}"  
                @click="displace=index" :key="index">
                <div class="dot_inside"></div>
            </div>
            <div v-if="arrows" @click="displace++" class="arr arr_left">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 972 1858">
                    <defs/>
                    <path id="Фигура_1" data-name="Фигура 1" class="cls-1" d="M42.819,1857.19l-42.43-42.43L928.82,886.326l42.431,42.431Z"/>
                    <path id="Фигура_1_копия" data-name="Фигура 1 копия" class="cls-1" d="M-0.364,42L42.067-.435,970.5,928l-42.431,42.43Z"/>
                </svg>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'ColorfullSlider',
    data(){
        return{
            Nodes:[],
            maxSlide: 0,
            displace: 0,
            wheelLoading: false
        }
    },
    props:{
        captions: {
            type: Array,
            default: ()=> ['']
        },
        sidebar: {
            type: Boolean,
            default: ()=> false
        },
        arrows: {
            type: Boolean,
            default: ()=> false
        },
        dots: {
            type: Boolean,
            default: ()=> true
        },
        autoplay: {
            type: Boolean,
            default: ()=> false
        },
        autoplaySpeed: {
            type: Number,
            default: ()=> 3000
        },
        light:{
            type: Boolean,
            default: ()=> true
        },
        wheelActive:{
            type: Boolean,
            default: ()=> true
        }
    },
    updated() {
        try{
            for(let i in this.$refs.brushes){
                this.$refs.brushes[i].classList = ['brush']
                if (i >= this.maxSlide-this.displace+1 ){
                    this.$refs.brushes[i].classList = 'brush brush_out_vision_big' 
                }
                if (i <= this.maxSlide-this.displace-3){
                    this.$refs.brushes[i].classList = 'brush brush_out_vision_little' 
                }
            }
            this.$refs.brushes[this.maxSlide-this.displace].classList.add('brush_current_slide')
            this.$refs.brushes[this.maxSlide-this.displace-1].classList.add('brush_current_slide-1')
            this.$refs.brushes[this.maxSlide-this.displace-2].classList.add('brush_current_slide-2')
        }
        catch{
            console.log
        }
    },
    mounted(){
        
        try{
            this.Nodes = [...this.$refs.gallery.childNodes]
        }catch{
            console.log
        }
        this.fillCaptions()
        this.displace = this.maxSlide = this.Nodes.length-1
        
        window.addEventListener('keyup', (e)=>{
            if (e.keyCode === 40||e.keyCode === 39){
                this.displace-- //ближе
            }
            if (e.keyCode === 38||e.keyCode === 37){
                this.displace++ //дальше
            }
        })
        this.$refs.gallery
            .addEventListener('wheel', 
                this.wheel
            )
        if (this.autoplay){
            setInterval(() => {
                this.displace--
            }, this.autoplaySpeed);
        }
        this.rewrite()
    },
    methods:{
        rewrite(){
            for(let i in this.Nodes){
                this.Nodes[i].classList = ['slide']
                if (i >= this.maxSlide-this.displace+1 ){
                    this.Nodes[i].classList = 'slide out_vision_big' 
                }
                if (i <= this.maxSlide-this.displace-3){
                    this.Nodes[i].classList = 'slide out_vision_little' 
                }
            }
            
            try{
                this.Nodes[this.maxSlide-this.displace].classList.add('current_slide')
                setTimeout(()=>{
                    this.Nodes[this.maxSlide-this.displace].classList= 'slide current_slide z-index'
                },700)
                this.Nodes[this.maxSlide-this.displace-1].classList.add('current_slide-1')
                this.Nodes[this.maxSlide-this.displace-2].classList.add('current_slide-2')
            }
            catch{
                console.log
            }
        },
        
        wheel(e){
            if (!this.wheelActive) return
            e.preventDefault();
            this.$refs.gallery.removeEventListener('wheel', this.wheel)
            this.wheelLoading = true
            setTimeout(()=>{
                this.wheelLoading = false
                this.$refs.gallery.addEventListener('wheel', this.wheel,{ once: true })
            }, 2000)
            let mouseDirection = Math.sign(e.deltaY)
            if (mouseDirection>0) this.displace--
            if (mouseDirection<=0) this.displace++
        },
        fillCaptions(){
            for(let i in this.Nodes){
                if (!this.captions[i]){
                    this.captions.splice(i, 1, `Slide ${+i+1}`)
                }
            }
            this.captions=this.captions.reverse()
            console.log(this.captions)
        }
    },
    destroyed() {
        this.$refs.gallery.removeEventListener('wheel', this.wheel)
    },

    watch:{
        displace(){
            this.rewrite()
            if (this.maxSlide-this.displace<0) {
                this.displace = 0
            }
            if (this.maxSlide-this.displace>this.maxSlide){
                this.displace = this.maxSlide
            }
        }
    }
}
</script>

<style lang="css" scoped>
    .brush{
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        transition: all 1s ease-in-out;
    }
    .brush img {
        width: 100%;
        height: 100%;
    }
    .fade-enter-active,
    .fade-leave-active {
        transition: opacity 0.5s ease;
    }
    .invert{
        filter: invert(1)
    }
    .fade-enter-from,
    .fade-leave-to {
        opacity: 0;
    }
    .slide{
        width: 100%;
        height: 100%;
        /* background-color: rgba(255, 0, 0, 0.452); */
        transition: all 1s ease-in-out;
    }
    .ColorfullGalleryWrapper{
        width: 100%;
        height: 100%;
        position: relative;
        overflow: hidden;
        background-color: transparent
    }
    .z-index{
        z-index: 3;
    }
    .current_slide{
        transform: scale(1);
        backdrop-filter: blur(2px);
    }
    .current_slide-1{
        /* background-color: rgba(255, 0, 140, 0.452); */
        transform: scale(0.8);
    }
    .current_slide-2{
        transform: scale(0.6); 
        /* background-color: rgba(89, 0, 255, 0.452); */
    }
    .on_current{
        transform: scale(1.3);
        opacity: 0;
    }
    .out_vision_big{
        opacity: 0;
        transform: scale(1.3);
    }
    .out_vision_little{
        opacity: 0;
        transform: scale(0.4);
    }




    .brush_current_slide{
        transform: scale(2.5);
    }
    .brush_current_slide-1{
        transform: scale(2);
    }
    .brush_current_slide-2{
        transform: scale(1.5); 
    }
    .brush_on_current{
        transform: scale(3);
        opacity: 0;
    }
    .brush_out_vision_big{
        opacity: 0;
        transform: scale(4);
    }
    .brush_out_vision_little{
        opacity: 0;
        transform: scale(1);
    }






    .dots{
        position: absolute;
        bottom: 10px;
        left: calc(50% - 175px);
        width: 350px;
        display: flex;
        z-index: 10;
        transition: all ease-in-out .3s;
        flex-direction: row-reverse;
        justify-content: center;
        align-items: center;
    }
    .dot{

        width: 15px;
        cursor: pointer;
        height: 15px;
        position: relative;
        border-radius: 50%;
        margin: 4px;
        background-color: transparent;
        border: 2px solid rgba(0, 0, 0, 0.856);
        transition: all .2s, ease-in;
        overflow: hidden;
    }
    .dot:hover{
        opacity: .5;
        
    }
    .slide_wrapper{
        height: 100%;
    }
    .dot_inside{
        content: '';
        width: 6px;
        position: absolute;
        height: 6px;
        border-radius: 50%;
        top: calc(100% + 5px);
        left: calc(50% - 3px);
        transition: all ease-in-out .3s;
        background-color: black;
    }
    .dotActive .dot_inside{
        content: '';
        width: 6px;
        position: absolute;
        height: 6px;
        border-radius: 50%;
        top: calc(50% - 3px);
        left: calc(50% - 3px);
        background-color: black;
    }
    .arr{
        margin: 15px 5px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
    }
    .arr:hover{
        transform: scale(1.2)
    }
    .arr_left:hover{
        transform: scale(1.2) rotate(180deg)
    }
    .arr_left{
        transform: rotate(180deg);
    }
    .sidebar{
        width: 70px;
        position: absolute;
        display: flex;
        justify-content: space-around;
        flex-direction: column-reverse;
        top: 0;
        height: 100%;
        right: 0;
        opacity: .6;
        transition: all ease-in .3s;
        background: rgb(154,158,251);
        background: linear-gradient(90deg, rgba(255, 255, 255, 0) 10%, rgba(0, 0, 0, 0.548) 120%);
        z-index: 100;
    }
    .sidebar:hover{
        opacity: 1;
        width: 80px;
    }
    .sidebar_item_wrapper{
        position: relative;
    }
    .sidebar_item{
        position: relative;
    }
    .sidebar_caption_line{
        margin-left: 10px;
        width: 25px;
        height: 10px;
        border-radius: 2px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.356);
        position: absolute;
        right: -24px;
        top: 4px;
    }
    .sidebar_caption{
        transition: all .2s ease-in;
        position: relative;
        text-align: right;
        margin-left: -100px ;
        padding: 5px;
        transform: translateX(-14px);
        color: rgba(255, 255, 255, 0.2);
        cursor: pointer;
    }
    .sidebar_caption:hover{
        transform: translateX(-20px);
        color: rgba(255, 255, 255, 0.8);
    }
    .current_caption{
        color: rgb(255, 255, 255);
        transform: translateX(-20px);
    }

    .wheel_loading{
        position: absolute;
        bottom: 25px;
        right: 100px;
        display: flex;
        z-index: 1000;
    }
    .wheel_loading_item{
        width: 6px;
        height: 6px;
        border-radius: 50%;
        background-color: white;
        margin: 2px;
        
    }
    @keyframes wheel_loading {
        0%{
            transform: translateY(0px);
        }
        100%{
            transform: translateY(5px);
        }
    }
    .wheel_loading_item_1{
        animation: wheel_loading .3s ease-in-out 0s infinite alternate forwards;
    }
    .wheel_loading_item_2{
        animation: wheel_loading .3s ease-in-out .2s infinite alternate forwards;
    }
    .wheel_loading_item_3{
        animation: wheel_loading .3s ease-in-out .4s infinite alternate forwards;
    }
    
</style>