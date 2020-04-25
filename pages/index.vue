<template>
  <div>
    <b-container fluid>
      <b-row>
        <b-col cols="2">
          <b-form-datepicker v-model="date" locale="en-IN" />
          <b-list-group v-if="archive" class="py-3">
            <b-list-group-item
              v-for="(title, i) in Object.keys(archive)"
              :key="i"
              :active="activeTitle == title"
              @click="activeTitle = title"
              href="#"
            >
              {{ title }}
            </b-list-group-item>
          </b-list-group>
        </b-col>
        <b-col v-if="archive" cols="10">
          <b-card-group columns>
            <b-card
              v-for="(article, i) in articles"
              :key="i"
              :title="article.title"
              :img-src="article.urlToImage"
              :img-alt="article.title"
              img-top
              tag="article"
              style="max-width: 20rem;"
              class="mb-4"
            >
              <b-card-text>
                <span class="small d-block my-2">
                  {{ article.source.name }}
                </span>
                {{ article.description }}
              </b-card-text>
              <b-row>
                <b-col>
                  <b-link :href="article.url" class="card-link" target="_blank">
                    Read more >>
                  </b-link>
                </b-col>
              </b-row>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      archive: null,
      activeTitle: null,
      date: ''
    }
  },
  computed: {
    articles() {
      return this.archive[this.activeTitle]
    }
  },
  watch: {
    date: {
      handler(af) {
        this.getArchive()
      }
    }
  },
  mounted() {
    this.getArchive()
  },
  methods: {
    async getArchive() {
      this.archive = null
      this.activeTitle = null
      if (this.date.length > 0) {
        const url = '/data/' + this.date + '/archive.json'
        try {
          this.archive = await this.$axios.$get(url)
          this.activeTitle = Object.keys(this.archive)[0]
        } catch (error) {
          alert('No archive found for ' + this.date)
        }
      }
    }
  }
}
</script>
