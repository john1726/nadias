<template>
    <form class="item-form" @submit.prevent="save">
        <div>
            <input type="text" placeholder="Item name" v-model="item.name" required>
            $<input type="number" min="0" step=".01" v-model="item.price" required>
        </div>
        <div>
            <textarea v-model="item.description" placeholder="Item description" required></textarea>
        </div>
        <div>
            <select v-model="item.category_id" required>
                <option value="">Select a category</option>
                <option v-for="cat in initialCategories" :value="cat.id" :key="cat.id">{{cat.name}}</option>
            </select>
        </div>        
        <button type="submit">Save</button>
        <ul>
            <li v-for="(error, index) in errors" :key="index">{{error}}</li>
        </ul>
    </form>
</template>

<script>
    export default {
        props: ['initial-categories'],
        data() {
            return {
                item: {
                    name: '',
                    price: 0.00,
                    image: '',
                    category_id: '',
                    description: ''
                },
                errors: []
            };
        },
        methods: {
            save() {

            }
        }
    }
</script>