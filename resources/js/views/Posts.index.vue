<template>
    <div>
        
        <div class="container grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-8 p-6">
            <PostCard v-for="post in posts" :key="post.id" 
                :post="post"/>
        </div>
        <div class="container">
            <ul class="flex gap-5 justify-center items-center">
                <li v-for="n in lastPage" :key="n" 
                    @click="fetchPostsData(n)"
                    :class="[ currentPage === n ? 'bg-amber-600' : 'bg-slate-600','pageLink p-4 rounded-full h-8 w-8 flex items-center justify-center cursor-pointer']">
                    {{ n }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import PostCard from '../components/PostCard.vue'

export default {

    components: {
        PostCard,
    },

    data() {
        return{
            posts: [],
            lastPage: 0,
            currentPage: 1,
        }
    },

    methods: {
        fetchPostsData(page = 1) { //questo parametro corrispondera al numero della pagina nell'URL, es. /api/posts?page=1
            axios.get('/api/posts',{
                params: {
                    page: page,
                }
            })
            .then( res => {
                // console.log(res.data.posts);
                this.posts = res.data.posts.data;
                this.lastPage = res.data.posts.last_page;
                this.currentPage = res.data.posts.current_page;
                console.log(this.posts);
            })
            .catch( err => {
                console.warn(err);
            })
        }
    },

    mounted() {
        this.fetchPostsData();
    }
}
</script>

<style lang="scss" scoped>

</style>