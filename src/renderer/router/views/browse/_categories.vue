<template lang="pug">
.page-container
	// categories
	ma-section(:title='$tc("category", 0)')

		.section-items-container
			ma-item(
        v-for='category in categories.items',
        type="category",
        :key='category.id',
        :image='category.icons[0].url',
        :title='category.name')
</template>

<script>
export default {
  data() {
    return {
      categories: [],
    };
  },
  created() {
    // fetch the data when the view is created and the data is
    // already being observed
    this.getCategories();
  },
  methods: {
    // get categories from the api
    getCategories() {
      const that = this;
      const country = that.$store.state.currentUser.country;

      that.$startLoading('fetching data');
      that.axios({
        method: 'get',
        url: '/browse/categories',
        params: {
          country,
        },
      }).then((res) => {
        that.categories = res.data.categories;
        that.$endLoading('fetching data');
      }).catch(() => {
        that.$router.go(-1);
        that.$endLoading('fetching data');
        that.$store.commit('ADD_NOTICE', that.$t('errors.fetchcategories'));
      });
    },
  },
};
</script>
