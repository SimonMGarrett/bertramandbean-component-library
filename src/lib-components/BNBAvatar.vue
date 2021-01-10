<template>
  <div class="bnb-avatar flex">
    <img
      class="rounded-full w-8 h-8 m-2"
      :src="require(`~/assets/img/${article.author.img}`)"
      alt="Author"
    />
    <div class="info relative -top-1 my-2 pt-0 text-75">
      <div class="text-gray-600 font-bold">{{ article.author.name }}</div>
      <div v-if="article.createdAt" class="text-gray-600">
        {{ $formatDate(article.createdAt) }}
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
.relative {
  position: relative;
}
.-top-1 {
  top: -1px;
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
.pt-0 {
  padding-top: 0;
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
