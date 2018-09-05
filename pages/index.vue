<template>
  <v-layout column justify-center align-center>
    <v-container grid-list-sm fluid>
      <v-layout row wrap justify-center align-center>
        <v-text-field
        v-model="query"
        append-icon="search"
        placeholder="Search"
        ></v-text-field>
      </v-layout>
      <v-layout row wrap>
        <v-flex xs12 md4>

        </v-flex>
        <v-flex xs12>
          <no-ssr>
            <ais-index :search-store="searchStore" :query="query" :auto-search='true'>
              <ais-range-input attribute-name="price"></ais-range-input>
              <ais-results inline-template :results-per-page='50'>
                <v-layout row wrap>
                  <v-flex xs6 md3 v-for="result in results" :key="result.objectID">
                    <v-card height="350px" class="pa-2" hover flat nuxt :to="$route.path+'/' + 'linku-i-qart' + '?itemId='">
                      <v-layout row wrap>
                        <v-flex xs12 class="pt-3 pb-2">
                          <v-img
                          :src="result.profileImg"
                          height="200"

                          class="pt-2"
                          :alt="result.title"
                          >
                        </v-img>
                        </v-flex>
                      </v-layout>
                      <v-layout row wrap>
                        <v-flex xs12 style="height: 70px">
                          <v-card-title class="pa-0 pt-2">{{ result.title }}</v-card-title>
                          <span class="hidden-sm-and-up">€ {{ result.price }}</span>
                        </v-flex>
                        <v-flex xs12 d-inline-flex>
                          <v-flex xs4 class="text-xs-left">
                            <v-btn flat icon color="red">
                              <v-icon color="red">far fa-heart</v-icon>
                            </v-btn>
                          </v-flex>
                          <v-spacer class="hidden-sm-and-up"></v-spacer>
                          <v-flex xs4 class="text-xs-right" >
                            <v-btn flat icon color="primary">
                              <v-icon color="primary">add_shopping_cart</v-icon>
                            </v-btn>
                          </v-flex>
                        </v-flex>
                      </v-layout>
                    </v-card>
                  </v-flex>
                </v-layout>
              </ais-results>
            </ais-index>
          </no-ssr>

        </v-flex>
      </v-layout>
    </v-container>
  </v-layout>
</template>
<script>

import { createFromAlgoliaCredentials, createFromSerialized, FACET_OR } from 'vue-instantsearch'
let store

export default {
  name: 'main-index',
  layout: 'tepazari',
  asyncData({ context, route }) {
    store = createFromAlgoliaCredentials(
      'YBLQTBT6Z9',
      '70dba138cca84e25c38f60fc89c16000'
    )
    store.indexName = 'cars-kosovo'
    store.addFacet('price', FACET_OR)
    store.highlightPreTag = '<mark>'
    store.highlightPostTag = '</mark>'
    store.start()
    store.refresh()
    return store.waitUntilInSync().then(() => {
      // eslint-disable-next-line no-param-reassign
      return { serializedSearchStore: store.serialize() }
    })
  },
  data () {
    return {
      searchStore: store,
      query: ''
    }
  },
  compoents: {

  },
  computed: {

  },
  created () {
    this.searchStore = createFromSerialized(this.serializedSearchStore)
  },
  methods: {
    onPageChange (page) {
      window.scrollTo(0, 0)
    }
  }
}
</script>
