<template>
  <a
    class="bnb-link"
    :href="path"
    :style="`color: ${color}; text-decoration: ${textDecoration}`"
    @click.prevent="propRouter.push(path)"
  >
    <slot />
  </a>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: '#888',
    },
    textDecoration: {
      type: String,
      default: 'underline',
    },
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
            return (location.href = path);
          },
        };
      },
    },
  },
};
</script>
