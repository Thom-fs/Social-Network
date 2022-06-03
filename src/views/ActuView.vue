<template>
  <Postcontainer />
</template>

<script>
import Postcontainer from "@/components/Postcontainer.vue";

export default {
  components: {
    Postcontainer,
  },
};
</script>

<style scoped>
h1 {
  color: #474e58;
}

.containerActu {
  width: 85%;
}

.mainContainerActu {
  display: flex;
  position: relative;
  top: 8px;
}
</style>
