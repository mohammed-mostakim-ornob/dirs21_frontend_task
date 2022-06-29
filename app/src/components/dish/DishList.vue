<template>
    <div>
        <div class="row">
            <div class="col-md-8">
                <h1 class="float-left">Dish Console</h1>
            </div>
            <div class="col-md-4">
                <button type="button" class="btn btn-danger w-100"
                    @click="onClickClearAll">Clear All Dishes</button>
            </div>
        </div>
        <dish-list-item v-for="dish in allDishes" :key="dish._id" :dish="dish"></dish-list-item>
    </div>
</template>

<script>
import DishListItem from './DishListItem.vue'
import { mapGetters, mapActions } from 'vuex'
import storeNamespaces from '../../store/namespaces'
import * as dishStoreTypes from '../../store/modules/dish/types'

export default {
	name: 'DishList',
	components: {
		DishListItem
	},
    async created() {
        const isSuccessful = await this.fetchAllDishes()

        if (!isSuccessful)
            this.$alertify.error('Dishes could not be loaded')
    },
    methods: {
        ...mapActions(storeNamespaces.dish, {
            fetchAllDishes: dishStoreTypes.actions.fetchAllDishes,
            clearAllDishes: dishStoreTypes.actions.clearAllDishes
        }),
        onClickClearAll: async function() {
            const isSuccessful = await this.clearAllDishes()

            if (isSuccessful)
                this.$alertify.success('All dishes successfully deleted')
            else
                this.$alertify.error('All dishes could not be deleted')
        }
    },
    computed: {
        ...mapGetters(storeNamespaces.dish, {
            allDishes: dishStoreTypes.getters.allDishes
        })
    }
}
</script>