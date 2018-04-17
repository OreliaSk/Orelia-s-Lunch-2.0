<template>
    <main class="menu">
        <div class="row">
            <div class="col-md-4 menu__1">
                <table class="table">
                    <thead class="thead-dark">
                        <tr>
                        <th scope="col">Entrées</th>
                        <th scope="col">Prix</th>
                        <th scope="col">Quantité</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.entries" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>{{menu.qty}}</td>
                        </tr>
                    </tbody>
                </table>

                <table class="table">
                    <thead class="thead-dark">
                    <tr>
                        <th scope="col">Boissons</th>
                    </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.drinks" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>{{menu.qty}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="col-md-4 menu__2">
                <table class="table">
                    <thead class="thead-dark">
                        <tr>
                        <th scope="col">Plats</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.dishes" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>{{menu.qty}}</td>
                        </tr>
                    </tbody>
                </table>

                <table class="table">
                    <thead class="thead-dark">
                    <tr>
                        <th scope="col">Desserts</th>
                    </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.desserts" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>{{menu.qty}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="col-md-4 commande">
                <table class="table">
                    <thead class="thead-dark">
                        <tr>
                        <th scope="col">Votre commande</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr> 
                            <td>Mark</td>
                            <td>Otto</td>
                            <td>@mdo</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </main>
</template>

<script>
    export default{
        data(){
            return{
                menus:[]
            }
        },
        created(){
            this.fetchMenus();
        },
        methods:{
            fetchMenus(){
                axios.get('api/menus').then((res) =>{
                    this.menus = res.data;
                    console.log(this.menus);
                });
            }
        }
    }
</script>