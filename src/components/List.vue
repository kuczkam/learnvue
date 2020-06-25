<template>
<div class="list-wrapper">
    <div class="list-item" v-for="(post, index) in postsList" :key="index">
        <h1>Title</h1>
        <p>{{ post.title }}</p>
        <h1>Body</h1>
        <p>{{ post.body }}</p>

        <button @click="showComments($event)" :data-post-id="post.id">poka komentarze</button>

        <Comments v-if="post.id == postIndex" :commentsList="comments">
            <label>Dodaj koment:</label>
            <TextInput label="podaj imię" v-model="name"/>
            <TextInput label="podaj email" v-model="email"/>
            <TextInput label="podaj treść" v-model="body"/>

            <button @click="createComment()">dodaj gówno</button>
        </Comments>
    </div>
</div>
</template>

<script>
import Comments from './Comments.vue'
import TextInput from './TextInput.vue'

export default {
    components: {
        Comments,
        TextInput
    }, 
    props: [
        'postsList'
    ],
    data() {
        return {
            comments: [],
            postIndex: '',
            name: '',
            email: '',
            body: '',
            commentsBlock: false
        }
    },
    methods: {
        getPostComments(id) {
            fetch(`https://gorest.co.in/public-api/comments?post_id=${id}&access-token=vmgYixsJh_zUBRutPwQcC_q1IMnU38lEiU4n`, {
                method: 'get'
            })
            .then((response) => {
                return response.json()
            })
            .then((jsonData) => {
                this.comments = jsonData.result
            })
        },
        showComments(e) {
            let id = e.target.getAttribute("data-post-id")
            this.getPostComments(id)
            this.postIndex = id
            this.commentsBlock = true
        },
        createComment() {
            fetch('https://gorest.co.in/public-api/comments', {
                method: 'post',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer vmgYixsJh_zUBRutPwQcC_q1IMnU38lEiU4n'
                },
                body: JSON.stringify({
                    post_id: this.postIndex, 
                    name: this.name,
                    email: this.email, 
                    body: this.body
                })
            }).then(function(response) {
                if (response.ok) {
                    alert('dodane')
                }
            })         
        },      
    },
}
</script>

<style scoped>
.list-wrapper {
    width: 500px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin: auto;
}

.list-item {
    margin: 10px;
    background: #ccc;
    padding: 40px;
}

p {
    color: #fff
}

h1 {
    font-size: 20px;
}
</style>