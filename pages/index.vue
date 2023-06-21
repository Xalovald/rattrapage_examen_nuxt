<script setup>

import ProductCard from '~/components/productCard.vue';

const products = ref([]);
const chosenProducts = ref([]);
const searchQuery = ref("");
const loading = ref(true)

async function getProducts() {

    const apiProducts = await fetch('https://dummyjson.com/products')
    const jsonProducts = await apiProducts.json()
    products.value = jsonProducts.products
    loading.value = false
}

function addProductChosen(product) {
    chosenProducts.value.push(product)
}

const calcTotal = computed(() => {
    return chosenProducts.value.reduce((acc, product) => acc + product.price, 0)
})

const filteredProducts = computed(() => {
            if (!searchQuery) {
                return products.value;
            }
            return products.value.filter((product) => {
                return product.title.toLowerCase().includes(searchQuery.value.toLowerCase());
            });
        },
)

onMounted(() => {
    loading.value = true
    getProducts()
})
</script>
<template>
    <div class="navbar navbar-dark z-20 fixed flex items-center">
        <div class="flex flex-row twelve columns items-center justify-end">
            <div class="nav-link columns">
                <div class="flex justify-end">
                    <div class="dropdown mr-52 ">
                        <a class="button button-danger text-center"><IconCSS size="20" name="uil:shopping-cart"/></a>
                        <div class="dropdown-content height-300-pixels overflow-y-scroll">
                            <hr v-show="chosenProducts.length > 0">
                            <div v-for="product in chosenProducts">
                                <p class="text-base">{{ product.title }}</p>
                                <hr>
                            </div>
                            <div class="flex justify-between">
                                <p>TOTAL</p>
                                <p>{{ calcTotal }} €</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div> 
    <div class="pt-32">
        <div v-if="loading" class="flex justify-center items-center">
            <Icon name="svg-spinners:12-dots-scale-rotate" size="50" class="color-black"/>
        </div>
        <div v-else class="flex flex-wrap gap-10 justify-center">
            <ProductCard @chooseProduct="addProductChosen" v-for="product in filteredProducts" :card="product"/>
        </div>
    </div>
</template>