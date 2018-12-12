<template>
  <div class="post-list">
    <section class="list-view">
      <div
        v-if="loading"
        class="loading">loading...</div>
      <div
        v-else-if="postsList.length === 0"
        class="no-content">nothing...</div>
      <ol
        v-else
        class="list">
        <li
          v-for="{ title, id } in postsList"
          :key="id"
          class="list-item">
          <router-link 
            :to="'/post/' + id"
            class="item-title">
            {{ title }}
          </router-link>
        </li>
      </ol>
    </section>
    <b-pagination align="center" :total-rows="100" v-model="currentPage" :per-page="10">
    </b-pagination>
  </div>
  
</template>

<script>
export default {
  name: 'PostList',
  data() {
    return {
      currentPage: 1,
      postsList: [
        {
          id: 1,
          title: 'Post #1'
        },
        {
          id: 2,
          title: 'Post #2'
        },
        {
          id: 3,
          title: 'Post #3'
        },
        {
          id: 4,
          title: 'Post #4'
        },
      ],
      loading: false
    }
  }
}
</script>

<style scoped>
.list-view ol, ul {
  padding: 0;
  list-style: none;
}

.list-item {
  position: relative;
  margin-bottom: 50px;
  border-bottom: 1px solid #ddd;
}

.item-title {
  display: inline-block;
  margin-bottom: 10px;
  font-size: 16px;
  color: #555;
}

.item-title:hover {
  color: #08c;
}

</style>


