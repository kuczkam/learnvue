<template>
    <div class="search-container">
        <input type="text" autocomplete="off"
        v-model="searchUser"
        @input="filterUsers"
        @focus="modal = true"
        :placeholder="placeholder" >
        <div v-if="filteredUsers && modal" class="test">
            <ul>
                <li v-for="filteredUser in filteredUsers" 
                :key="filteredUser.id"
                @click="setUser(filteredUser)" >
                    {{ filteredUser.first_name }}
                </li>
            </ul>
        </div>
        <div>
            <List :postsList="loadPosts" />
            <button v-if="this.postLimit < getTotalPostsNum()" @click="showMore()">Więcej gówna</button>
        </div>
    </div>
</template>

<script>
import List from './List.vue'

export default {
    components: {
        List,
    },
    props: [
        'value', 
        'placeholder',
    ], 
    data() {
        return {
            searchUser: '',
            users: [],
            posts: [],
            filteredUsers: [],
            modal: false,
            postLimit: 1,
            totalPosts: 0,
        }
    },
    mounted() {
        this.getUsers()
    },
    methods: {
        getUsers() {
            fetch('https://gorest.co.in/public-api/users?access-token=vmgYixsJh_zUBRutPwQcC_q1IMnU38lEiU4n', {
                method: 'get'
            })
            .then((response) => {
                return response.json()
            })
            .then((jsonData) => {
                this.users = jsonData.result
                if (this.searchUser.length == 0) {
                    this.filteredUsers = this.users
                }
            })
        },
        getUserPosts(userId) {
            fetch(`https://gorest.co.in/public-api/posts?user_id=${userId}&access-token=vmgYixsJh_zUBRutPwQcC_q1IMnU38lEiU4n`, {
                method: 'get'
            })
            .then((response) => {
                return response.json()
            })
            .then((jsonData) => {
                this.posts = jsonData.result
                this.postId = this.posts[0].id
            })
        },                             
        filterUsers() {
            this.filteredUsers = this.users.filter((searchUser) => {
                return searchUser.first_name.toLowerCase().startsWith(this.searchUser.toLowerCase())
            })
        },
        setUser(user) {
            this.searchUser = user.first_name
            this.getUserPosts(user.id)
            this.modal = false
        },
        showMore() {
            this.postLimit += 1
        },
        getTotalPostsNum() {
            this.totalPosts = this.posts.length
            return this.totalPosts
        }
    },
    computed: {
        loadPosts() {
            return this.posts.slice(0, this.postLimit)
        }
    }
}
</script>

<style>
    li {
        list-style: none;
        cursor: pointer;
        padding: 4px;
    }
</style>