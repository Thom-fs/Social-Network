<template>
  <div class="content">
    <EditPost />
    <Posts class="comments" />
  </div>
</template>

<script>
import Posts from "@/components/Posts.vue";
import EditPost from "@/components/EditPost.vue";

export default {
  name: "",
  components: {
    Posts,
    EditPost,
  },
};
</script>

<style scoped>
.content {
  padding: 4% 0;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
