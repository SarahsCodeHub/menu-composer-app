<template>
    <li class="col-span-1 flex rounded-md shadow-sm mb-3">
        <div
            class="flex-shrink-0 flex items-center justify-center w-16 bg-indigo-600 text-white text-sm font-medium rounded-l-md">
            {{ dish.preparationTimeInMinutes | minutes('short') }}</div>
        <div class="flex flex-1 items-center justify-between truncate border-t  border-b border-gray-200 bg-white">
            <div class="flex-1 truncate px-4 py-2 text-sm">
                <label :for="`dish-${dish.id}`" class="font-medium text-gray-900 hover:text-gray-600">
                    {{ dish.name }}
                </label>
                <p :id="`dish-${dish.id}-price`" class="text-gray-500">{{ dish.priceInEuro | currency }}</p>
            </div>
            <div class="flex-shrink-0 pr-2">
                <button type="button" @click="showDetails = true"
                    class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-white bg-transparent text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
                    <span class="sr-only">Zeige Details</span>
                    <!-- Heroicon name: mini/ellipsis-vertical -->
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
                    </svg>


                </button>
            </div>
        </div>
        <div class="flex-shrink-0 flex items-center justify-center text-white text-sm font-medium rounded-r-md border-t border-r border-b border-gray-200"
            :class="cardType === 'option' ? 'w-16' : 'w-1'">
            <input v-if="cardType === 'option'" v-model="checked" @change="addToOrRemoveFromMenu" :id="dish.id"
                :name="dish.name" type="checkbox"
                class="h-4 w-4 rounded border-gray-400 text-indigo-600 focus:ring-indigo-500">
        </div>
        <dish-detail :showModal="showDetails" :dish="dish" @close-details="showDetails = false"
            @add-dish-to-menu="addToMenu">
        </dish-detail>
    </li>
</template>

<script>
import eventBus from "../../utils/eventBus";
import DishDetail from './Detail.vue'
export default {
    name: "dish-card",
    components: {
        DishDetail,
    },
    props: {
        dish: {
            type: Object,
            default: () => { }
        },
        cardType: {
            type: String,
            default: 'option'
        }
    },
    data() {
        return {
            checked: false,
            showDetails: false
        }
    },
    methods: {
        addToOrRemoveFromMenu() {
            if (this.checked) {
                eventBus.$emit('add-dish-to-menu', this.dish)
            } else {
                eventBus.$emit('remove-dish-from-menu', this.dish.id)
            }
        },
        addToMenu() {
            eventBus.$emit('add-dish-to-menu', this.dish)
            this.checked = true
        }
    }
}
</script>
