<template>
    <form @submit.prevent="saveCategories">    
        <a @click="addCategory" class="add">+ Add Category</a>    
        <div v-for="(category, index) in categories" :key="category.id">
            <input type="text" :value="category.name" @input="update($event, 'name', index)" :ref="category.name">
            <input type="number" :value="category.display_order" @input="update($event, 'display_order', index)">
            <a @click="removeCategory(index)" class="remove">delete</a>
            <div>
                <img v-if="category.image" :src="`/images/${category.image}`" width="100">
                <label v-else>Image: </label>
                <input type="text" :value="category.image" @change="update($event, 'image', index)">
            </div>
            <hr>
        </div>
        <button type="submit">Save</button>
        <div>{{ feedback }}</div>
    </form>
</template>

<script>
    export default {        
        data() {
            return {                
                feedback: ''
            };
        },
        computed: {
            categories() {
                return this.$store.state.categories;
            }
        },
        methods: {
            removeCategory(index) {
                if (confirm('Are you sure?')) {
                    let id = this.categories[index].id;
                    if (id > 0) {
                        axios.delete('/api/categories/' + id);
                    }
                    this.categories.splice(index, 1);
                }
            },
            addCategory() {
                this.$store.commit('ADD_CATEGORY', {
                    id: 0,
                    name: '',
                    image: '',
                    display_order: this.categories.length + 1
                });
                this.$nextTick(() => {
                    window.scrollTo(0, document.body.scrollHeight);
                    this.$refs[''][0].focus();
                });
            },
            saveCategories() {
                axios.post('/api/categories/upsert', {
                    categories: this.categories
                })
                .then((res) => {
                    if (res.data.success) {
                        this.feedback = 'Changes saved.';
                        this.categories = res.data.categories;
                    }
                });
            },
            update($event, property, index) {
                this.$store.commit('UPDATE_CATEGORY', {
                    index,
                    property,
                    value: $event.target.value
                });
            }
        }
    }
</script>

<style scoped>
    hr {
        margin-bottom: 30px;
    }
    img {
        vertical-align: middle;
    }
</style>