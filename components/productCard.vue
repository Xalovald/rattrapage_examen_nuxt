<script setup>

defineProps({

    card: {

        type: String,

        required: true
    }
})

const chosen = defineEmits(["chooseProduct"])

function chooseProduct(product) {
    chosen('chooseProduct', product)
}
</script>
<template>
    <div class=" width-400-pixels height-500-pixels bg-green-400 rounded-xl color-white">
        <img :src="card.thumbnail" class="width-100-percents height-40-percents object-contain">
        <div class="flex flex-column justify-between height-60-percents p-5">
            <h3 class="font-bold">{{ card.title }}</h3>
            <p class="text-xl"> {{ card.description }}</p>
            <div class="flex flex-row justify-between items-center">
                <button class="button button-danger" @click="chooseProduct(card)">ajouter au panier <IconCSS size="20" name="uil:plus"/></button>
                <p> {{ card.price }} &euro;</p>
            </div>
        </div>
    </div>
</template>