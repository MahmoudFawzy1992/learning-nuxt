<template>
    <div class="admin-post-page">
        <section class="update-form">
        <adminPostForm :post="loadedPost" @submit="onSubmitted"/>
        </section>
    </div>
</template>

<script>
import AdminPostForm from '@/components/Admin/AdminPostForm'
import axios from 'axios'
export default {
    layout: 'admin',
    components: {
        AdminPostForm,
    },
    asyncData(context) {
        return axios
        .get(
            "https://nuxt-blog-e0788.firebaseio.com/posts/" + context.params.postId + ".json"
        )
        .then (res => {
            return {
                loadedPost: res.data
            }
        })
        .catch (e => context.error());
    },
    methods: {
        onSubmitted(editedPost) {
            axios.put("https://nuxt-blog-e0788.firebaseio.com/posts/" + this.$route.params.postId + ".json", editedPost)
            .then(res=> this.$router.push('/'))
            .catch(e=> console.log(e))
        }
    }
   

}
</script>
<style scoped>
.update-form {
    width: 90%;
    margin: 20px auto;
}
@media (min-width: 768px) {
    .update-form {
        width: 500px;
    }
}
</style>
