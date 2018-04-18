<template>
    <main>
        <div class="home" v-if="selected">
            <div class="d-flex"> 
                <div class="home__img col-md-6">
                    <img :src="'images/couple-diner.jpg'" alt="couple-diner" class="home__img">
                </div>
                <div class="home__text col-md-5">
                    <div class="title text-center">
                        Besoin d'une pause dèj ?<br>
                        Tu as frappé à la bonne porte !
                    </div>
                    <button class="
                    btn btn-default 
                    home__button 
                    d-block 
                    mx-auto" 
                    v-bind:class="{active : isActive}"
                    v-on:click="getMenu"
                    >
                    J'ai faim !
                    </button>
                </div>
            </div>
        </div>
        <div v-if="!selected">
            <MenuComponent></MenuComponent>
        </div>
    </main>
</template>

<script>
export default {

    data(){
        return{
        selected: true,
        isActive: true
        }
    },

    mounted() {
      this.classChanges();
    },

    methods: {
        classInterval(){
            this.isActive = !this.isActive;
        },
        classChanges(){
            setInterval(this.classInterval, 1000);
        },
        getMenu() {
            this.selected = false;
        }
    },
}
</script>