<template>
  <v-main>
    <v-container fluid>
      <v-row v-if="$fetchState.pending" align-content="center" justify="center">
        <v-col cols="12" class="subtitle-1 text-center">
          درحال دریافت اطلاعات ...
        </v-col>

        <v-col cols="11">
          <v-progress-linear
            indeterminate
            rounded
            color="blue"
          ></v-progress-linear>
        </v-col>
      </v-row>

      <v-row
        v-else-if="$fetchState.error"
        align-content="center"
        justify="center"
      >
        <v-col cols="12" class="subtitle-1 text-center">
          متاسفانه مشکلی پیش آمده!
        </v-col>

        <v-col cols="11">
          <v-progress-linear
            color="red lighten-2"
            buffer-value="25"
            reverse
            stream
          ></v-progress-linear>
        </v-col>

        <v-col cols="11">
          <v-btn outlined depressed color="red lighten-2" block @click="$fetch">
            تلاش مجدد
            <v-icon right dark> mdi-refresh </v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-row v-else>
        <sidebar-pc-index v-if="!mobile" />

        <sidebar-mobile-index v-if="mobile" />
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  async fetch() {
    await this.$store.dispatch('sidebar/getApiSidebars')
  },
  head: {
    titleTemplate: '%s | آیتم های منو',
  },
  computed: {
    mobile() {
      return this.$vuetify.breakpoint.smAndDown
    },
  },
  mounted() {
    this.$store.commit('navbar/updateNav', 'آیتم های منو')
  },
  fetchOnServer: false,
}
</script>
