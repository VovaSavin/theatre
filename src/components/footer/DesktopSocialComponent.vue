<template>
  <div
    v-if="!isMobile"
    class="c_main_white cancel_reverse"
    :class="{ d_flex_row: left, d_flex_row_reverse: !left }"
  >
    <span
      class="f_oswald f_size_24 lower_case c_pointer"
      @click="socialLink(socLink)"
      :class="{ nav_link_color: color }"
    >
      {{ socName }}
    </span>
  </div>
</template>
      <script>
export default {
  name: "DesktopSocialComponent",
  props: {
    socLink: String,
    socName: String,
    color: Boolean,
    left: Boolean,
  },
  components: {},
  data() {
    return {
      isMobile: false,
    };
  },
  created() {},
  methods: {
    socialLink(lnk) {
      // Anchor to top
      window.open(lnk, "_blank").focus();
    },
  },
};
</script>
<style scoped>
@media screen and (max-width: 1000px) {
  .cancel_reverse {
    flex-direction: row !important;
    justify-content: center;
  }
}

.c_main_white {
  color: #f9f9f9;
}

.c_main_white:hover {
  color: #e0e0e0;
}
.f_size_24 {
  font-size: 1.3em;
  font-weight: 200;
}
</style>