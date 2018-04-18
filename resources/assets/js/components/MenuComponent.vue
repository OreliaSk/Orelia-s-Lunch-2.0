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
                        <th scope="col">Validation</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.entries" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>
                                <div class="btn btn-primary" @click="addToCart(menu)">Ajouter</div>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <table class="table">
                    <thead class="thead-dark">
                    <tr>
                        <th scope="col">Boissons</th>
                        <th scope="col">Prix</th>
                        <th scope="col">Quantité</th>
                        <th scope="col">Validation</th>
                    </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.drinks" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>
                            </td>
                            <td>
                                <div class="btn btn-primary" @click="addToCart(menu)">Ajouter</div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="col-md-4 menu__2">
                <table class="table">
                    <thead class="thead-dark">
                        <tr>
                        <th scope="col">Plats</th>
                        <th scope="col">Prix</th>
                        <th scope="col">Quantité</th>
                        <th scope="col">Validation</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.dishes" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>
                                <div class="btn btn-primary" @click="addToCart(menu)">Ajouter</div>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <table class="table">
                    <thead class="thead-dark">
                    <tr>
                        <th scope="col">Desserts</th>
                        <th scope="col">Prix</th>
                        <th scope="col">Quantité</th>
                        <th scope="col">Validation</th>
                    </tr>
                    </thead>
                    <tbody>
                        <tr v-for="menu in menus.desserts" v-bind:key="menu.id"> 
                            <td>{{menu.name}}</td>
                            <td>{{menu.price}}€</td>
                            <td>
                                <div class="btn btn-primary" @click="addToCart(menu)">Ajouter</div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="col-md-4 commande">
                <table class="table">
                    <thead>
                        <tr>
                            <th scope="col">Récapitulatif de votre commande</th>
                            <th>Prix à l'unité</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="product in products" v-bind:key="product.id">
                            <td>{{ product.name }} </td>
                            <td>{{product.price }}€</td>
                            <td>
                                <button v-on:click="removeItem(product)"><i class="fa fa-trash-o"></i></button>
                            </td>
                            <td>
                                <button v-on:click="minusOne(product)"><i class="fa fa-caret-square-o-down"></i></button> 
                                <button v-on:click="plusOne(product)"><i class="fa fa-caret-square-o-up"></i></button>
                                <span class="badge">{{ product.qty }}</span>
                            </td>
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
        </div>
 
    </main>
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