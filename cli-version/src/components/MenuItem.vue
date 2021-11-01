
<template>
    <div class="menu-item">
                <img
                    class="menu-item__image"
                    v-bind:src="image.source"
                    v-bind:alt="image.alt"
                />
                <div>
                    <h3>{{ name }}</h3>
                    <p>Prix: {{ generatePrice }}</p>
                    <p v-if="inStock === true">En stock</p>
                    <p v-else>En rupture de stock</p>
                    <div>
                        <label for="add-item-quantity">Quantité : {{ quantity }}</label>
                        <input v-model.number="quantity" id="add-item-quantity" type="number"/>
                        <BaseButton @click="updateShoppingCart( quantity)">
                            Ajouter au panier d'achat
                        </BaseButton>
                    </div>
                </div>
            </div>
</template>


<script>
import BaseButton from "./BaseButton.vue"
import { mapActions } from 'vuex'

    export default {
	name: "MenuItem",
	components: {
		BaseButton
	},
        props: {
            image: {
                type: Object,
                required: true
            },
            inStock: {
                type: Boolean,
                required: true
            },
            name: {
                type: String,
                requierd: true
            },
            quantity: {
                type: Number,
                required: true
            },
            price: {
                type: Number,
                required: true
            }
        },
        data() {
            return {
                onSale: false
            }
        },
        computed: {
            generatePrice() {
                if (this.onSale) {
                    return (this.price * 0.9).toFixed(2)
                } else {
                    return this.price
                }
            }
        },
        beforeMount() {
            const tody = new Date().getDate();

            if (tody % 2 === 0) {
                this.onSale = true
            }
        },
        methods: {
            ...mapActions(["updateShoppingCart"])
            // updateShoppingCart(amount) {
            //     this.$emit('add-items-to-cart', amount)
            // }
        }
    }
</script>

<style>

.menu-item {
  display: flex;
  width: 500px;
  justify-content: space-between;
  margin-bottom: 30px;
}
.menu-item__image {
  max-width: 300px;
}
</style>