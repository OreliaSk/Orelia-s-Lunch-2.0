<template>
    <div>
        <div class="menu container-fluid p-0">
            <h3 class="menu__header text-center">Faites vous plaisir, choisissez tout ce qui vous fait envie !</h3>
                <div class="menu__body container">
                    <div class="row">

                        <!-- Bloc menu 1 -->
                        <div class="menu__choice col-md-4">

                            <!-- Entrées -->
                            <table class="table">
                                <thead>
                                    <tr>
                                    <th scope="col">
                                        <img :src="'images/salad.png'" alt="entrée" class="menu-icone">
                                        Entrées
                                    </th>
                                    <th></th>
                                    <th></th>
                                    <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="menu in menus.entries" v-bind:key="menu.id">
                                        <td>
                                            <label>{{menu.name}}</label>
                                        </td>
                                        <td>{{ menu.price }}€</td>
                                        <td>
                                        </td>
                                        <td><button class="btn btn-default" @click="addToCart(menu)">ok</button></td>
                                    </tr>
                                </tbody>
                            </table> <!-- fin entrée -->
                            

                            <!-- Boissons -->
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th scope="col">
                                            <img :src="'images/cocktail.png'" alt="boissons" class="menu-icone">
                                            Boissons
                                        </th>
                                        <th></th>
                                        <th></th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="menu in menus.drinks" v-bind:key="menu.id">
                                        <td>
                                            <label>{{menu.name}}</label>
                                        </td>
                                        <td>{{ menu.price }}€</td>
                                        <td>
                                        </td>
                                        <td><button class="btn btn-default" @click="addToCart(menu)">ok</button></td>
                                    </tr>
                                </tbody>
                            </table> <!-- fin boissons -->
                        </div> <!-- Fin bloc menu 1 -->

                        <!-- Bloc menu 2-->
                        <div class="menu__choice col-md-4">

                            <!-- Plats -->
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th scope="col">
                                            <img :src="'images/hamburger.png'" alt="plats" class="menu-icone">
                                            Plats
                                        </th>
                                        <th></th>
                                        <th></th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="menu in menus.dishes" v-bind:key="menu.id">
                                        <td>
                                            <label>{{menu.name}}</label>
                                        </td>
                                        <td>{{ menu.price }}€</td>
                                        <td>
                                        </td>
                                        <td><button class="btn btn-default" @click="addToCart(menu)">ok</button></td>
                                    </tr>
                                </tbody>
                            </table> <!-- fin plats -->
                            

                            <!-- Desserts -->
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th scope="col">
                                            <img :src="'images/muffin.png'" alt="boissons" class="menu-icone">
                                            Desserts
                                        </th>
                                        <th></th>
                                        <th></th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="menu in menus.desserts" v-bind:key="menu.id">
                                        <td>
                                            <label>{{menu.name}}</label>
                                        </td>
                                        <td>{{ menu.price }}€</td>
                                        <td>
                                        </td>
                                        <td><button class="btn btn-default" @click="addToCart(menu)">ok</button></td>
                                    </tr>
                                </tbody>
                            </table> <!-- fin desserts -->
                        </div> <!-- Fin bloc menu 2-->
                        

                        <!-- Bloc commande recap' -->
                        <div class="menu__choice col-md-4">
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th scope="col">Commande</th>
                                        <th>Prix à l'unité</th>
                                        <th>Qté</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="product in products" v-bind:key="product.id">
                                        <td>{{ product.name }} </td>
                                        <td>{{product.price }}€</td>
                                        <td>{{product.qty}}</td>
                                        <td>
                                            <span v-on:click="removeItem(product)" style="cursor:pointer"><i class="fa fa-trash-o"></i></span>
                                            <span v-on:click="minusOne(product)" style="cursor:pointer"><i class="fa fa-caret-square-o-down"></i></span> 
                                            <span v-on:click="plusOne(product)" style="cursor:pointer"><i class="fa fa-caret-square-o-up"></i></span>
                                        </td>
                                    </tr>
                                    <tr>
                                        <td class="col-md-6">Total :</td>
                                        <td class="card-footer text-muted">
                                            {{productsTotal}}€
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div> <!-- Fin bloc commande recap' -->
                    </div> <!-- Fin row -->
                </div> <!-- Fin menu__body -->
        </div> <!-- Fin menu -->

        <FooterMenu></FooterMenu>

    </div>
</template>

<script>
    export default{

        data(){
            return{
                menus:[],
                products:[],
            }
        },
        created(){
            this.fetchMenus();
        },
        
        computed: {
            productsTotal: function() {     
            let total = 0;
            let product;
            
            for (product in this.products) {
                total = total + (this.products[product].qty * this.products[product].price);
            }
            return total;
            }
        },
            
        methods: {
            fetchMenus(){
                axios.get('api/menus').then((res) =>{
                    this.menus = res.data;
                });
            },

            addToCart: function(menu) {
                this.products.push({
                    id: menu.id,
                    name: menu.name,
                    price: menu.price,
                    qty: 1
                });
                //this.menus.splice(menu, 1);
            },

            plusOne: function(product) {
                product.qty = product.qty + 1;
            },

            minusOne: function(product) {
                if (product.qty == 1) {
                    this.removeItem(product);
                } else {
                    product.qty = product.qty - 1;
                }
            },
            
            removeItem: function(product) {
                this.products.splice(product, 1);   
            },    
        }
    }

</script>