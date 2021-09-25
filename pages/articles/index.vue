<template>
  <div>
    <v-container>
      <v-row>
        <div v-for="b in blogs" :key="b.slug">
          <v-col>
            <v-card class="mx-auto" max-width="344" outlined>
              <v-list-item three-line>
                <v-list-item-content>
                  <!-- <div class="text-overline mb-4">OVERLINE</div> -->
                  <v-list-item-title class="text-h5 mb-1">
                    {{ b.title }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{
                    b.description
                  }}</v-list-item-subtitle>
                </v-list-item-content>

                <v-list-item-avatar
                  tile
                  size="80"
                  color="grey"
                ></v-list-item-avatar>
              </v-list-item>

              <v-card-actions>
                <!-- <v-btn outlined rounded text :to="'/articles/' + b.slug"> -->
                <!-- <v-btn outlined rounded text :to="b.dir + '/' + b.slug"> -->
                <v-btn outlined rounded text :to="b.path">
                  Button
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </div>
      </v-row>
    </v-container>

    <!-- <nuxt-link :to="'/articles/' + b.slug">
        {{ b.title }} {{ b.description }}
      </nuxt-link> -->
  </div>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    // const query = await $content('articles' || 'index',{ deep: true }).limit(10)
    const query = await $content('nuxt' || 'index',{ deep: true }).limit(10)
    const blogs = await query.fetch()
    return { blogs }
  },
}
</script>
