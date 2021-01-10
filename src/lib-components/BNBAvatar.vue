<template>
  <div class="bnb-avatar flex relative">
    <img
      v-if="article.author.img"
      class="rounded-full w-8 h-8 m-2"
      :src="article.author.img"
      alt="Author"
    />
    <div class="my-2 pt2 text-75">
      <div v-if="article.author.name" class="text-gray-600 font-bold">
        {{ article.author.name }}
      </div>
      <div v-if="article.createdAt" class="text-gray-600">
        {{ article.createdAt }}
      </div>
    </div>

    <!-- TODO - follow links or some other CTA -->
  </div>
</template>

<script>
export default {
  props: {
    article: {
      type: Object,
      default: () => {
        return {
          createdAt: null,
          author: {
            img: '',
            name: '',
          },
        };
      },
    },
  },
};
</script>

<style>
.bnb-avatar {
  font-family: sans-serif;
}
.relative {
  position: relative;
}
.top-2 {
  top: 2px;
}
.flex {
  display: flex;
}
.w-8 {
  width: 2rem;
}
.h-8 {
  height: 2rem;
}
.rounded-full {
  border-radius: 9999px;
}
.m-2 {
  margin: 0.5rem;
}
.my-2 {
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}
.pt2 {
  padding-top: 2.25px;
}
.text-75 {
  font-size: 0.75rem;
}
.text-gray-600 {
  --text-opacity: 1;
  color: #718096;
  color: rgba(113, 128, 150, var(--text-opacity));
}
.font-bold {
  font-weight: 700;
}
</style>
