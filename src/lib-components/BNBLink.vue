<template>
  <a :href="path" @click.prevent="propRouter.push(path)">
    <slot />
  </a>
</template>

<script>
export default {
  props: {
    path: {
      type: String,
      default: '/',
    },
    propRouter: {
      type: Object,
      default: () => {
        return {
          // Whichever object you push (though it's likely to be Vue.$router),
          // you need to ensure it has a push(<string>) method.
          push: (path) => {
            return (location.path = path);
          },
        };
      },
    },
  },
};
</script>
