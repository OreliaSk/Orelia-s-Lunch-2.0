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
                            </table>
                            <!-- fin entrée -->

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
                            </table>
                            <!-- fin boissons -->
                        <!-- Fin bloc menu 1 -->
                        </div>

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
                            </table>
                            <!-- fin plats -->

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
                            </table>
                            <!-- fin desserts -->

                        </div>
                        <!-- Fin bloc menu 2-->

                        <!-- Bloc commande recap' -->
                        <div class="menu__choice col-md-4">
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th scope="col"></th>
                                        <th>Récapitulatif de votre commande</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="product in products" v-bind:key="product.id">
                                        <td>{{ product.name }} </td>
                                            <td>{{product.price }}€</td>
                                            <td>
                                                <button v-on:click="removeItem(product)" class="btn btn-danger"><i class="fa fa-trash-o"></i></button>
                                            </td>
                                            <td>
                                                <button v-on:click="minusOne(product)" class="btn btn-primary"><i class="fa fa-caret-square-o-down"></i></button> 
                                            </td>
                                            <td><button v-on:click="plusOne(product)" class="btn btn-primary"><i class="fa fa-caret-square-o-up"></i></button></td>
                                    </tr>
                                    <tr>
                                        <div class="total">
                                        <div class="row">
                                            <p class="col-md-6">Total :</p>
                                            <p class="card-footer text-muted">
                                                {{productsTotal}}€
                                            </p>
                                        </div>
                                    </div>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <!-- Fin bloc commande recap' -->

                    </div> <!-- Fin row -->
                </div> <!-- Fin menu__body -->
        </div>
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
                total = total + (this.products[product].quantity * this.products[product].price);
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
                    quantity: 1
            });
            this.menus.splice(menu, 1);
            },
            plusOne: function(product) {
                product.quantity = product.quantity + 1;
            },
            minusOne: function(product) {
                if (product.quantity == 1) {
                    this.removeItem(product);
                } else {
                    product.quantity = product.quantity - 1;
                }
            },
            removeItem: function(product) {
                this.menus.push({
                    id: product.id,
                    name: product.name,
                    price: product.price
                    });
                this.products.splice(product, 1);   
            },    
        }
    }

</script>